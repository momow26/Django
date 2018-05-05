# Django学习第2天(01)

------
### 目录
在前三小结的基础上，这里重新创建一个stu应用app，并将models模块进行分析与代码的整理
> * stu项目的创建
> * 在models创建stu模型
> * 数据的迁移
> * 执行迁移
> * 创建超级管理员账户
> * 注册的2种方法及页面内容的修改
> * Django 模型字段参考

### stu项目的创建

详细方法见前文中myapp的创建，与myapp一模一样的，只是是不同的应用程序，相互之间并列，名称不同而已

### 在models创建stu模型

在models.py文件中定义class模型名称
继承models.Model
也就是相当于在models模块下创建一个类，在类下定义不同的方法
```
from django.db import models

### 定义一个Student类
class Student(models.Model):
    # 数据的保持 有姓名和性别属性
	s_name =  models.CharField(max_length=20)
	s_gender = models.BooleanField()
    # db_tables:定义数据库中的表名称，如果在这里不这么定义，就会在数据库中显示为db_table_Student
	class Meta:
		db_table = 'stu'
* stu的创建这里的属性字段类型有：
CharField（）
BooleanField()
```
------		

### 数据库的迁移
1.前面的设置中已经定义好了数据库的名称
2.原理：生成数据库文件
3.生成迁移文件
命令：python manage.py makemigrations

------		

### 执行数据库的迁移
1.在前一步的基础上，执行如下命令，进行文件的迁移。
2.原理：将数据迁移到数据库中
3.完成迁移任务
命令：python manage.py migrate

### 创建超级管理员账户
1.在管理后台（admin中）添加学生，就i需要创建管理员账户-密码
命令：python manage.py createsuperuser
然后设置账户，邮箱，密码，重复验证密码（密码不能太过于简单）
```
python manage.py createsuperuser
Username: wangmomo
Email address:momow26@163.com
Password:wangmomo
```
2.在MySQL数据库中查看，即可后台看到账户和密码
auth_user一栏中即可看到信息
3.登录后台，就看到了刚刚新建的User和admin账户
Users即为刚刚创建的用户
4.在新建的stu项目文件下的admin.py文件下，注册模型，在后台可以对定义的stu模型进行增删查改
```
# 需要先导入自己在models中创建的学生Student类
from django.contrib import admin
from stu.models import Student

# Register your models here.
admin.site.register(Student)
```
### 注册的2种方法及页面内容的修改
1.自己在admin.py中定义一个类，把学生列表给展示出来
```
from django.contrib import admin
from stu.models import Student


# Register your models here.
class StudentAdmin(admin.ModelAdmin):
    # 列表，只展示id和name字段，同时把StudentAdmin注册进去
    list_display = ['id', 's_name', 's_gender']
    
admin.site.register(Student, StudentAdmin)
```
2.这里需要定义一个方法，将性别为默认的1和0改为“男”和“女”
在class StudentAdmin(admin.ModelAdmin):中定义
```
    def set_s_gender(self):
        if self.s_gender:
            return '男'
        else:
            return '女'
```
3.一些页面修饰的命令
```
from django.contrib import admin
from stu.models import Student

# Register your models here.
class StudentAdmin(admin.ModelAdmin):
    # 设置性别为男女显示
    def set_s_gender(self):
        if self.s_gender:
            return '男'
        else:
            return '女'
    # 修改性别字段的描述
    set_s_gender.short_description = '性别'
    # 展示字段列表，只展示id和name字段，同时把StudentAdmin注册进去
    list_display = ['id', 's_name', set_s_gender]
    # 过滤，相当于只展示自己想要的那部分信息，过滤部分单独显示
    list_filter = ['s_name']
    # 搜索--多了一个搜索框
    search_fields = ['s_name']
    # 分页---分2页，每页2条内容
    list_per_page = 2
    

# 这里是注册的第一种方式
admin.site.register(Student, StudentAdmin)    
```
4.注册的另一种方法---装饰器方式@xxx
在定义好的类中做一个装饰器，来注册
```
from django.contrib import admin
from stu.models import Student

# Register your models here.
#注册的另一种方法---装饰器方式@xxx
@admin.register(Student)
class StudentAdmin(admin.ModelAdmin):
    # 设置性别为男女显示
    def set_s_gender(self):
        if self.s_gender:
            return '男'
        else:
            return '女'
    # 修改性别字段的描述
    set_s_gender.short_description = '性别'
    # 展示字段列表，只展示id和name字段，同时把StudentAdmin注册进去
    list_display = ['id', 's_name', set_s_gender]
    # 过滤，相当于只展示自己想要的那部分信息，过滤部分单独显示
    list_filter = ['s_name']
    # 搜索--多了一个搜索框
    search_fields = ['s_name']
    # 分页---分2页，每页2条内容
    list_per_page = 2
```
