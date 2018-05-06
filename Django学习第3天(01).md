# Django学习第3天(01)

------
### 目录

> * 班级表、学生表的创建
> * 班级表、学生表的的关联
> * 学生与班级相对应的关联

完成下面的工作：
创建班级表，学生表
1.url--->有一个班级页面，展示班级信息--->跳转--->
2.展示所有学生信息页面

### 班级表、学生表的创建
1.新建一个grade项目，具体这里不在详述，请参考前面内容
2. 定义all_grade方法、all_stu方法
```
def all_grade(request):
    grades = Grade.objects.all()
    return render(request, 'grades.html', {'grades': grades})
```
```
def all_stu(request):
    students = Student.objects.all()
    return render(request, 'students.html', {'students': students})
```
3.配置url路由
主路由
```
from django.conf.urls import url, include
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^stu/', include('stu.urls')),
    url(r'^grade/', include('grade.urls')),
]
```
grade路由
```
from django.conf.urls import url
from grade import views

urlpatterns = [
    url(r'^all_grade/', views.all_grade)
]
```
stu路由
```
from django.conf.urls import url
from stu import views

urlpatterns = [
    url(r'^all_stu/', views.all_stu),
]
```
4.创建students和grades的templates文件（即html文件）
grades文件---班级列表
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>班级列表</title>
</head>
<body>
    {% for grade in grades %}
        班级ID：{{ grade.id }}
        班级名称：{{ grade.g_name }}
        <br>
    {% endfor %}
</body>
</html>
```
students文件---学生页面
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>学生页面</title>
</head>
<body>
    {% for student in students %}
        学生ID: {{ student.id }}
        学生姓名：{{ student.s_name }}
        学生年龄：{{ student.s_age }}
        班级ID：{{ student.g_id }}
        <br>
    {% endfor %} 
</body>
</html>
```
### 班级表、学生表的的关联
1.点击班级表，跳转到班级下的学生页面
```
# 获取所有学生信息
def all_stu(request):
    students = Student.objects.all()
    return render(request, 'students.html', {'students': students})
```
相对应的html文件内容
```
{% for grade in grades %}
        班级ID：{{ grade.id }}
        # 这里增加了一个超链接，从班级表链接到学生表
        <a href="/stu/all_stu/">
            班级名称：{{ grade.g_name }}
        </a>
        <br>
    {% endfor %}
```
2.点击班级表，跳转到班级下的学生页面（具体班级id）
```
# 通过班级id，获取具体学生信息
def all_stu(request):
    students = Student.objects.filter(g_id=3)
    return render(request, 'students.html', {'students': students})
```
```
{% for grade in grades %}
        班级ID：{{ grade.id }}
        # 这里增加了一个超链接，从班级表链接到学生表
        <a href="/stu/all_stu/">
            班级名称：{{ grade.g_name }}
        </a>
        <br>
    {% endfor %}
```
### 学生与班级相对应的关联
1.学生的信息中要加入班级的id与之对应
```
# ?g_id={{ grade.id }}
<body>
    {% for grade in grades %}
        班级ID：{{ grade.id }}
        <a href="/stu/all_stu/?g_id={{ grade.id }}">
            班级名称：{{ grade.g_name }}
        </a>
        <br>
    {% endfor %}
</body>
```
2.在学生的页面中获取班级的信息，返回查询结果不满足条件的所有信息
```
def all_stu(request):
    #用GET.get来接收班级表的信息
    g_id = request.GET.get('g_id')
    students = Student.objects.filter(g_id=g_id)
    return render(request, 'students.html', {'students': students})
```   
配套使用
```
    {% for grade in grades %}
        班级ID：{{ grade.id }}
        <a href="/stu/all_stu/?g_id={{ grade.id }}">
            班级名称：{{ grade.g_name }}
        </a>
        <br>
    {% endfor %}
```
3.exclude方法--- 返回查询结果不满足条件的所有信息
```
def all_stu(request):
    #用GET.get来接收班级表的信息
    g_id = request.GET.get('g_id')
    students = Student.objects.exclude(g_id=g_id)
    return render(request, 'students.html', {'students': students})
```   
配套使用
```
    {% for grade in grades %}
        班级ID：{{ grade.id }}
        <a href="/stu/all_stu/?g_id={{ grade.id }}">
            班级名称：{{ grade.g_name }}
        </a>
        <br>
    {% endfor %}
```

