# Django学习第14天(01）-项目部署
 
-----

> * mysql包的安装
> * 本地数据库同步到阿里云数据库
> * 在远程服务器创建一个自己的文件夹
> * 修改上传文件配置
> * 配置必备库
> * 运行程序 （测试环境下）

### mysql包的安装
1.先进行更新,更新原来的环境
```
sudo apt update
```
2.安装mysql数据库
```
apt install mysql-server mysql-client
```
3.安装过程中，设置密码，这里书我们之前数据库的密码
4.输入命令，进入mysql数据库，账户是---root，输入上一步输入的数据库密码
```
mysql -u root -p
```
5.设置远程访问mysql
a)先找到mysql.conf文件
b)访问顺序
```
cd /etc/
cd mysql
cd mysql.conf.d
```
c)编辑mysqld.cnf文件
```
vim mysqld.cnf
```
d)找到bind-address文件，因为该地址只能本地访问，将该文件注释即可
6.启用mysql数据库
a)切换MySQL数据库
```
show databases; # 看有哪些数据库
use mysql; #切换到MySQL数据库，使用mysql数据库 显示Database changed即可进行下一步编辑
```
b)输入以下2个命令---忽略大小写
```
# 'root'---数据库名
# '123456'---数据库密码
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root@123' WITH GRANT OPTION;
# 显示Query OK 即表示成功

flush privileges; 
# 显示Query OK 即表示成功
```
7.重启mysql，之前修改配置的目的是：使mysql生效的
```
# Centos的数据库书mariadb
# service mysql start 启动mysql
# service mysql stop  停止
# service mysql status 查看状态
service mysql restart
```
### 本地数据库同步到阿里云数据库
1.这里使用Navicat for MySQL
2.在阿里云服务器先创建一个和本地数据库一样名字的数据库名
这里需要指定数据库的编码（utf8   utf8_general_ci）
3.在工具栏--->数据传入，将本地数据库传输到阿里云数据库
### 在远程服务器创建一个自己的文件夹
1.创建文件夹
```
# 这里根据个人的习惯，可以创建在不同的目录下
cd home
# 在home下创建名为app的文件夹
mkdir app
cd app
# 创建3个文件夹，src文件夹存放代码放； conf存放配置文件；log用来查看日志信息；
mkdir log conf src
```
2.上传文件
```
# 在Xshell中的打开新建新建文件传输，即Xftp，输入阿里云密码
cd src
# 查看刚刚上传的文件，和本地的相同即可
```
### 修改上传文件配置
1. 进入刚刚上传的文件夹中进行文件配置的修改
2.修改DATABASES，即可访问云服务器中的数据库
```
# 这里是云服务器上面数据库的密码
'USER': 'root',
'PASSWORD': '123456',
# 这里设置为自己阿里云服务器的公网id地址
'HOST': '112.74.172.000'
```
3.DEBUG = True 和 FALSE的区别
```
# 在上线的时候，务必这样DEBUG = False，因为如果将设置为True的话，用户的体验会很差
DEBUG = True

# *表示任何人都可以访问
ALLOWED_HOSTS = ['*']
```
### 配置必备库
4.安装python3，这里提示需要安装python3-pip
```
# 安装pip3
apt install python3-pip

pip3 install django==1.11
pip3 isntall pymysql
pip3 isntall Pillow
```
### 运行程序 （测试环境下）
1.运行程序，指定端口，这里最好指定80
2.使用公网ip进行访问，后面需要加刚刚设置的80端口
```
python3 manage.py runserver 0.0.0.0:80
# 将自己的公网ip替换0.0.0.0
0.0.0.0 ---- 112.74.172.000:80  # 端口自动隐藏
```
3.如果无法访问时，要对settings中的HOST进行修改
```
# 在上面的修改配置文件中已经配置过
'HOST': '112.74.172.000'
```
4.查看进程命令
```
# 如果遇到端口冲突，
netstat -lntp 
# 如果端口被占用，需杀点进程，
kill -9 port
```
5.在settings中将DEBGU = True 修为为DEBUG = False
```

```
6.在settings中注释
```
#STATIC_URL = '/static/'
```
7.修改url配置，加入静态路径

```
# 还原
STATIC_URL = '/static/'
# 加入下面一句，与settings中的静态路径呼应
url(r'^static/(?P<path>.*)$', serve, {"document_root": settings.STATIC_ROOT}),
```


































