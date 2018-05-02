# Django学习第1天（02）

------


> * 第一个Django工程的创建
> * myproject新工程文件介绍
> * myproject项目（元项目）的环境配置
> * myproject项目的运行

## 第一个Django工程的创建
1.创建运行Django项目的虚拟环境（virtualenv）
此处不在详述，参见---**Django学习第1天**内容
2.创建第一个Django项目
cd 进入虚拟环境 testenv下的Scripts进行activate激活
见到如下所示后
C:\env-all\envtest\Scripts>activate
然后再cd到要创建myproject工程的文件夹目录下
创建一个名字为**myproject**的**工程文件**
django-admin startproject myproject
生成了一个myproject工程文件
## myproject新工程文件介绍
创建的的pyproject工程包括2部分，一个myproject项目，以及一个manage.py文件
1.**manage.py文件**

2.myproject项目
a --- __init__.py

b --- settings.py

c --- urls.py

d --- wsgi.py

## myproject项目（元项目）的环境配置
1.settings.py配置
a INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'stu', #这里添加自己创建的项目文件名
]
b TEMPLATES = ['DIRS': [os.path.join(BASE_DIR, 'templates')],]
c DATABASES = [
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': '数据库名称---momo_mysql',
        'USER': 'root',
        'PASSWORD':'123456',
        'HOST':'localhost',
        'PORT':'3306'
]
LANGUAGE_CODE = 'zh=hans'
TIME_ZONE = 'Asia/Shanghai'
d STATIC_URL = '/static/'
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'static')
]
e MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
**还有一些需要修改，后续继续补充、完善**

2.__init__.py配置
import pymysql
pymysql.install_as_MySQLdb()

## myproject项目的运行
python manage.py runserver 
注：runserver 后可以加ip:端口号 
--- python manage.py runserver ip:8080
然后再浏览器窗口输入127.0.0.1：8000 ---这里的端口默认为8000
结果显示如下，即为第一个Django工程文件顺利实现
