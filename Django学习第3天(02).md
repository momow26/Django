# Django学习第3天(02)

------
### 目录

> * 班级学生表关联概念
> * url路由
> * 定义学生拓展表类
> * 定义添加学生方法
> * 添加学生前端渲染页面
> * 写个方法，获取拓展表对应的学生
> * 通过学生信息，获取他/她的家庭信息

### 班级学生表关联概念
1.一对一  ---  1---1 --- OneToOneField 
主键和外键是一对一的关系，在关联表中，只能关联一个主表的id
拓展表找主表：拓展信息对象.关联字段
主表找拓展表的信息：主表对象.关联表的model_name

2.一对多 ---   1---n --- 见下次介绍
3.多对多 ---   m---n --- 见下次介绍

4.on_delete   on_delete=models.PROTECT 主表删除，从表也不能被删除
默认cascade，on_delete=models.CASCADE  主表删除，从表也删除
set_null 主表删除，从表关联字段设为空
protect 受保护的，不能删除
### url路由
```
from django.conf.urls import url
from stu import views
urlpatterns = [
    # 创建学生信息
    url(r'^add_students/', views.add_students),
]
```
### 定义学生拓展表类
```
class StudentInfo(models.Model):
    s_addr = models.CharField(max_length=100)
    # 与学生关联的，一对一
    stu = models.OneToOneField(Student)
    class Meta:
        db_table = 'stu_info'
```
### 定义添加学生方法
```
def add_students(request):
    if request.method == 'GET':
        return render(request, 'add_students.html')
    if request.method == 'POST':
        # 创建学生信息
        s_name = request.POST.get('name')
        s_gender = 1 if request.POST.get('gender') else 0
        s_birth = request.POST.get('birth')
        s_tel = request.POST.get('tel')
        s_Chinese = request.POST.get('Chinese')
        s_math = request.POST.get('math')
        Student.objects.create(
            s_name = s_name,
            s_gender = s_gender,
            s_birth = s_birth,
            s_tel = s_tel,
            s_Chinese = s_Chinese,
            s_math = s_math
        )
        return render(request, 'add_students.html')
```
### 添加学生前端渲染页面
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>添加学生信息</title>
</head>
<body>
    <form action="/stu/add_students/" method="post">
        <table>
            <tr>
                <td>姓名</td>
                <td>性别</td>
                <td>生日</td>
                <td>电话</td>
                <td>语文</td>
                <td>数学</td>
            </tr>
            <tr>
                <td><input type="text" name="name"></td>
                <td><input type="text" name="gender"></td>
                <td><input type="date" name="birth"></td>
                <td><input type="text" name="tel"></td>
                <td><input type="text" name="Chinese"></td>
                <td><input type="text" name="math"></td>
            </tr>
        </table>
        <input type="submit" value="提交">
    </form>
</body>
</html>
```
### 写个方法，获取拓展表对应的学生

通过拓展表学生的地址，来查找学生信息
```
url(r'^search_stu/', views.search_stu),
```
1.写法一， 但一般不这么写，需要查找2次数据库，会被老板打
```
def search_stu(request):
    # 通过拓展表学生的地址，来查找学生信息
    # 查找addr=北京的学生信息
    # 写法一， 但一般不这么写，需要查找2次数据库，会被老板打
    # stus = StudentInfo.objects.filter(s_addr='北京')
    # stu = stus[0]
    # search_stu = Student.objects.filter(id=stu.id)
    # return render(request, 'search_stu.html', {'search_stu': search_stu})
```
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    {% for i in search_stu %}
        姓名：{{ i.s_name }}
    {% endfor %}
</body>
</html>
```
2.写法二
```
stus = StudentInfo.objects.filter(s_addr__contains='北京')
    stu = stus[0]
    search_stu = stu.stu
    return render(request, 'search_stu.html', {'search_stu': search_stu})
```
这里查询不需要迭代，所有在html页面中去掉for循环
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    姓名：{{ search_stu.s_name }}
</body>
</html>
```

### 通过学生信息，获取他/她的家庭信息
```
stu = Student.objects.filter(s_name='白百合').first()
    search_stu = StudentInfo.objects.filter(id=stu.id)
    # search_stu = StudentInfo.objects.filter(stu_id=stu.id)
    return render(request, 'search_stu.html', {'search_stu': search_stu})
```

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    {% for i in search_stu %}
        地址：{{ i.s_addr }}
    {% endfor %}
</body>
</html>
```