# Django学习第1天(04)

------

在前三小结的基础上，这里重新创建一个stu应用app，并将models模块进行分析与代码的整理

> * stu的创建
> * stu中models的配置
> * 数据的迁移
> * 执行迁移

## stu的创建
详细方法见前文中myapp的创建，与myapp一模一样的，只是是不同的应用程序，相互之间并列，名称不同而已
------

## stu中models的配置
也就是相当于在models模块下创建一个类，在类下定义不同的方法
from django.db import models


class Student(models.Model):

	s_name = models.CharField(max_length=20)
	s_gender = models.BooleanField()

	class Meta:
		db_table = 'stu'
		
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

