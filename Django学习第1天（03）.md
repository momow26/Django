# Django学习第1天（03）

------

可以在pycharm中打开上节创建的代码，继续操作 

> * pycharm里虚拟环境的配置
> * pycharm里配置Debug
> * 创建第一个应用（app）
> * 在myapp应用中的views里定义方法
> * myproject项目下urls的配置
> * myapp项目下urls的配置


## pycharm里虚拟环境的配置
1.根据上一节的内容，将其内容移至到pycharm神器中继续操作，但在操作之前，需要配置pycharm中的环境
在Settings --->Project--->Project interpretor下，将之前创建好的带有python.exe的虚拟环境路径加载到此处，作为继续在pycharm上运行的python环境及virtualenv虚拟环境

## pycharm里配置Debug
**Debug**用来进行代码的调试
1.在pycharm里的Run菜单栏下的Debug下进行python解释器的配置
2.分别对Name,Scripts path和Parameters进行配置
Name：
Scripts path：
Parameters:

## 创建第一个应用（app）
1.在myproject工程下创建一个名为的应用
格式：python manage.py startapp app-name
这里就直接在pycharm中进行应用（app）的创建，与在cmd中命令相同
python manage.py startapp myapp
示例如下：
应用（app）里内容包括如下：
a __init__.py ：初始化
b admin.py ：管理后台注册模型
c apps.py ：settings环境里注册app的时候需要使用到。但是在这里不推荐使用
d models.py ：写模型的地方
e tests.py ：
f urls.py
g views.py ：写处理业务逻辑的地方
**强调：**这里使用的最多的有 d、f和g

## 在myapp应用中的views里定义方法
**这里使用HttpResponse的时候，需要导入如下模块** 
from django.http import HttpResponse


# 定义一个hello_girl的方法
def hello_girl(request):
    
    # 返回一个Hello, beautiful girl的页面对象
    return HttpResponse('Hello, beautiful girl')

## myproject项目下urls的配置
1.在myproject项目中的urls文件下配置urlpatterns
from django.conf.urls import url, include
from django.contrib import admin
from myapp import views

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^myapp/', include(myapp.urls)),
    # 这里的hello_girl为路径，通过它可以访问到hello_girl方法中的Hello, beautiful girl对象，**注意hello/---后面的/，代表路径的匹配**
    url(r'^hello_girl/', views.hello_girl),

## myapp项目下urls的配置
2.在myapp项目下新建urls文件夹，同时配置
该目录下的urls刚好匹配在myproject项目下urls路径的后面
from django.conf.urls import url
from myapp import views

urlpatterns = [
    url(r'^hello_girl/', views.hello_girl),
]
访问页面地址如下：
127.0.0.1:8000/myapp/hello_girl/
即会显示： Hello, beautiful girl
