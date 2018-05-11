# Django学习第14天(02）-Nginx-uwsgi配置Django项目
 
-----

> * 安装nginx
> * 安装uwsgi
> * 查看nginx运行状态
> * 创建、配置uwsgi
> * 启动uwsgi

### 安装nginx
1.基于ubantu系统
```
sudo apt install nginx
```
2.访问公网地址
```
# 显示如下代码即表示安装成功
Welcome to nginx!

If you see this page, the nginx web server is successfully installed and working. Further configuration is required.

For online documentation and support please refer to nginx.org.
Commercial support is available at nginx.com.

Thank you for using nginx.
```
### 安装uwsgi
1.命令
```
pip3 install uwsgi
```
### 查看nginx运行状态
1.命令
```
server nginx status
# 当出现绿色的 Active :active(running)时，表示运行正常
systemctl status nginx 查看nginx的状态
systemctl start/stop/enable/disable nginx 启动/关闭/设置开机启动/禁止开机启动
service nginx status/stop/restart/start
```
2.修改nginx配置文件
```
cd /etc/nigix
vim nginx.conf
# 这里不需要配置，自己写
```
3.在自己创建的nginx.conf文件中写信息
```
cd home/app/conf/
vim name_nginx.conf
server {
        listen          80;
        server_name 112.74.172.237 localhost;
        
        access_log /home/app/log/access.log;
        error_log /home/app/log/error.log;
        location / {
                        include uwsgi_params;
                        uwsgi_pass 127.0.0.1:8890;
                }
        location /static/ {
                                alias /home/app/day11axf0/static/;
                                expires 30d;
                }
        }
```
### 创建、配置uwsgi
```
# uwsgi.ini
[uwsgi]
master = true # 主进程
processes = 4 # 进程
pythonpath = '/home/app/src/momowang' # 指定项目目录
module = momowang.wsgi # 在工程目录下指定wsgi
# 此处的socket与上节中nginx的location中的uwsgi_pass 127.0.0.1:8890进行通信，关联起来
socket = 127.0.0.1:8890 # 指定端口，8890为任意设置
logto = /home/app/log/uwsgi.log # 指定日志写在哪里
```
5.将自己创建的nginx.conf文件，放在总的ngnix文件夹下，才可以生效
```
# 进入总nginx文件夹
vim /etc/nginx/nginx,conf
# 将自己的写的内容包含 *.表示全部包括
include /home/app/conf/*.conf;
```
6.重启nginx，使之生效
```
service nginx status
service nginx restart
# 如果重启失败，使用命名netstat -nltp 查看进程，如有80端口，则使用kill -p pid（进程号）
# 如果一次不成功，多次netstat -nltp查看进程，杀进程即可
```
### 启动uwsgi
1.启动命令
```
uwsgi --ini uwsgi.ini
```
2.查看
```
tail -f uwsgi.log
```



























