# Django学习第2天(03)

------
### 目录

> * 在stu中的views.py定义一个添加学生方法
> * 在templates模板中的新建一个index.html文件
> * 调试代码的一种方法
> * 在views.py定义添加完整学生信息的2中方法
> * 获取所有学生信息

### 在stu中的views.py定义一个添加学生方法
```
from django.http import HttpResponse
from django.shortcuts import render

# Create your views here.

# 添加学生信息
def add_stu(request):
    # 在方法种可以定义是get请求，还是post请求，
    # 这里需要post提交请求
    # 如果是get请求，返回index页面
    if request.method == 'GET':
        return render(request, 'index.html')
    # return HttpResponse('hello, blonde girl!')
    # 如果是post请求，则提交信息，在需要处理请求
    if request.method == 'POST':
        # 处理请求，处理提交的学生信息
        print(request)
        #上面返回的是---<WSGIRequest: POST '/stu/add_stu/'>
        print(request.POST)
        #上面返回的是一个字典---<QueryDict: {'name': ['刘明湘'], 'gender': ['女'], 'birth': ['1989-04-30'], 'tel': ['15111112222']}>
        return HttpResponse('添加学生信息成功')
```
### 在templates模板中的新建一个index.html文件
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <form action="/stu/add_stu/" method="post">
        <table>
            <tr>
                <td>姓名</td>
                <td>性别</td>
                <td>生日</td>
                <td>电话</td>
            </tr>
            <tr>
                <td><input type="text" name="name"></td>
                <td><input type="text" name="gender"></td>
                <td><input type="date" name="birth"></td>
                <td><input type="text" name="tel"></td>
            </tr>
        </table>
        <input type="submit" value="提交">
    </form>
</body>
</html>
```

### 调试代码的一种方法

1.一行一行的打印
```
    print(request)
    print(request.POST)
    print(1)
    s_name = request.POST.get('name')
    print(2)
    s_gender = request.POST.get('gender')
    print(3)
    s_birth = request.POST.get('birth')
    print(4)
    s_tel = request.POST.get('tel')
```
2.打断点调试（debug）

### 在views.py定义添加完整学生信息的2中方法
1.方法一
```
from django.http import HttpResponse
from django.shortcuts import render
from stu.models import Student

def add_stu(request):
    if request.method == 'POST':
        s_name = request.POST.get('name')
        s_gender = 1 if request.POST.get('gender') else 0
        s_birth = request.POST.get('birth')
        s_tel = request.POST.get('tel')
        # 方法1
        stu = Student()
        stu.s_name = s_name
        stu.s_gender = s_gender
        stu.s_birth = s_birth
        stu.s_tel = s_tel
        stu.save()
        return HttpResponse('添加学生信息成功')
```
2.方法二
```
from django.http import HttpResponse
from django.shortcuts import render
from stu.models import Student

def add_stu(request):
    if request.method == 'POST':
        s_name = request.POST.get('name')
        s_gender = 1 if request.POST.get('gender') else 0
        s_birth = request.POST.get('birth')
        s_tel = request.POST.get('tel')
        # 方法2 ---以后常用这种-学生类.objects对象.create方法
        # objects对象：通过模型.objects来实现对数据的CRUD操作
        Student.objects.create(
            s_name = s_name,
            s_gender = s_gender,
            s_birth = s_birth,
            s_tel = s_tel
        )
        return HttpResponse('添加学生信息成功')
```

### 获取所有学生信息--
1.配置url路由
```
from django.conf.urls import url
from stu import views

urlpatterns = [
    # 查询所有学生信息
    url(r'^sel_stu/', views.selectStu),
]
```
2.定义查询所有学生的方法
```
def selectStu(request):
    # 查询所有学生信息--等同于MySQL中的select * from student
    stus = Student.objects.all()
    # 需要返回一个页面，将返回的学生信息加载到页面,返回的字典给前端，让前端去解析即可
    return render(request, 'select_stu.html', {'stus': stus})
```
3.前端页面展示
在templates下新建一个select_stu.html文件，在文件中编辑
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>查询所有学生信息</title>
</head>
<body>
    {% for stu in stus %}
        姓名：{{ stu.s_name }}
        电话：{{ stu.s_tel }}
        性别：{{ stu.s_gender }}
        生日：{{ stu.s_birth }}
        <br>
    {% endfor %}
</body>
</html>
```
4.查询所有女生信息
---where，这里使用filter(过滤)，这里不能用get,因为get不能迭代
```
def selectStu(request):
    # 查询所有女生信息
    stu_fem = Student.objects.filter(s_gender=True)
    return render(request, 'select_stu.html', {'stu_fem': stu_fem})
```
5.查询id等于3的学生，注意，如果这里没有相对应的id，也会返回一个空的姓名
```
def selectStu(request):
    # 查询id等于3的学生
    stus = Student.objects.filter(id=3)
    return render(request, 'select_stu.html', {'stus': stus})
```
6.get和filter的区别
-1) get：返回一个满足条件的对象，没有满足条件的，则直接报DoesNotExist的异常；
-2) 如果查询结果有多个数据的话，就报MultiObjectsReturned。
-3) filter:返回满足条件的结果

7.查询id从大到小排列/排序
```
 # 查询id从大到小排序
    # stus = Student.objects.all().order_by('-id')
    # return render(request, 'select_stu.html', {'stus': stus})
```
8.查询id从小到大的排序
```
    # 查询id从小到大排序
    # stus = Student.objects.all().order_by('id')
    # return render(request, 'select_stu.html', {'stus': stus})
```
9.查询id最大的一个学生数据
```
    # 查询id最大的一个学生数据
    # stus = Student.objects.all().order_by('-id').first()
    # return render(request, 'detail.html', {'stus': stus})
```
10.查询id最小的一个学生数据
```
    # 查询id最小的一个学生数据
    # stus = Student.objects.all().order_by('-id').last()
    # return render(request, 'detail.html', {'stus': stus})
```
11. 查询男生有多少个
```
    # 查询男生有多少个
    # stus = Student.objects.filter(s_gender=True).count()
    # return render(request, 'detail.html', {'stus': stus})
```
12.查询所有90后男生的信息
```
    # 查询所有90后男生的信息
    stus = Student.objects.filter(s_gender=True).\
        filter(s_birth__gte='1990-01-01').\
        filter(s_birth__lt='2000-01-01')
    return render(request, 'select_stu.html', {'stus': stus})
```
13.查询姓蔡学生的数据
```
    stus = Student.objects.filter(s_name__startswith='蔡')
    return render(request, 'select_stu.html', {'stus': stus})  
```
14.查询姓名以“为”结尾学生的信息
```
    stus = Student.objects.filter(s_name__endswith='为')
    return render(request, 'select_stu.html', {'stus': stus})  
```
15.查询姓名包含‘佩’学生的信息
```
    stus = Student.objects.filter(s_name__contains='佩')
    return render(request, 'select_stu.html', {'stus': stus})
```
16.判断是否存在“李丹”的学生
----TypeError: 'bool' object is not iterable
```
    # 此处错误使用
    stus = Student.objects.filter(s_name='李丹').exists()
    return render(request, 'select_stu.html', {'stus': stus})
```
17.获取指定的多个id的值
```
    ids = [1, 3, 4]
    stus = Student.objects.filter(id__in=ids)
    return render(request, 'select_stu.html', {'stus': stus})
```
18.查询语文成绩大于数学成绩10分的学生
```
    stus = Student.objects.filter(s_math__gte=F('s_Chinese') + 10)
    return render(request, 'select_stu.html', {'stus': stus})  
```
19.查询数学成绩大于等于语文成绩的学生
```
    stus = Student.objects.filter(s_math__gte=F('s_Chinese'))
    return render(request,'select_stu.html', {'stus': stus})
  
```
20.查询学生姓名不叫李丹的，或者语文成绩大于80的学生  

```
    # ~Q代表非  |代表或
    stus = Student.objects.filter(~Q(s_name='李丹') | Q(s_Chinese__gt=80))
    return render(request, 'select_stu.html', {'stus': stus})   
```
21.查询学生姓名不叫李丹的，且语文成绩大于80的学生
```
    # ~Q代表非  &代表且
    stus = Student.objects.filter(~Q(s_name='李丹') & Q(s_Chinese__gt=80))
    return render(request, 'select_stu.html', {'stus': stus})   
```
## 一些关键字的总结：
```
# filter(): 返回满足条件的结果
# first(): 返回第一条数据
# last(): 返回最后一条数据
# count(): 求和
# gt：大于
# gte：大于等于
# lt：小于
# lte: 小于等于
# from django.db.models import F, Q
# F(): 
# Q():
# &: 且
# |: 或
```








修改字段名称
alter table table_name change 原名 修改后名



