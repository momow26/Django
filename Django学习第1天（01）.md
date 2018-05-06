# Django学习第1天（01---0423）

------

Django介绍：
**Django**是一个开放源代码的Web应用框架，由Python写成。采用了MTV的框架模式，即模型M，模板T和视图V。它最初是被开发来用于管理劳伦斯出版集团旗下的一些以新闻内容为主的网站的，即是CMS（内容管理系统）软件。


> * Virtualenv虚拟环境的创建
> * pymysql环境的配置
> * Django包的安装

## Virtualenv虚拟环境的创建
**每个项目**都需要有自己的运行环境，一个项目，往往是一个团队在开发，同时，每个项目安装的库都有很大的差别。因此，使用虚拟环境就会将不同的项目环境分隔开，
安装步骤：(这里使用的是python 3.6.5)
1.先创建一个新的文件夹---env-all
2.cmd进入该文件夹下
3.安装虚拟环境
pip install virtualenv
![cmd-markdown-logo](https://www.zybuluo.com/static/img/logo.png)
4.在文件夹env-all目录下，指定安装虚拟环境中python版本的方式
virtualenv --no-site-packages -p "自己电脑中python解释器的安装路径\python.exe"  创建虚拟环境的文件名
virtualenv --no-site-packages -p "C:\Program Files\Python36\python.exe" envtest
5.进入虚拟环境
cd进入创建好的虚拟环境文件夹下的Scripts文件
cd C:\env-all\envtest
6.激活虚拟环境（只有在激活虚拟环境后，才可以继续下一步工作）
cd C:\Scripts
激活命令：activate  （Linux下命令：suorce bin/activate）
退出命令：deactivate

## pymysql环境的配置
在上一步建立好的virtualenv虚拟环境中，继续在Scripts目录下，安装pymysql环境
pip install pymysql

## Django包的安装
1.将python环境创建在virtualenv虚拟环境中，在virtualenv环境下的Scripts目录下进行pip install django==1.11.4
2.检查django是否安装成功
在cmd中输入python进入python交互式环境
import django
django.get_version()
当交互式环境中显示
'1.11.4'
