title: Nginx搭建HTTP文件服务器
author: ergz
tags:
  - Nginx
categories:
  - linux
date: 2019-11-14 16:04:00
---
### 介绍
**Nginx(engine x)是一个高性能的HTTP和反向代理web服务器，同时能提供了IMAP/POP3/SMTP服务，Nginx的优点：**

##### 1.高性能，支持高并发连接
##### 2.低资源消耗，内存占用少
##### 3.稳定性高
##### 4.配置简单
##### 5.支持热部署
<!-- more -->

### 使用
#### 1.检查Nignx是否安装
**使用以下命令检查Nginx服务器状态：**
```bash
systemctl status nginx
```
#### 2.安装Nginx(ubuntu)
**命令：**
```bash
sudo apt-get install nginx
```
**Ubuntu安装之后的文件结构大致为：**
*	所有的配置文件都在/etc/nginx下，并且每个虚拟主机已经安排在了/etc/nginx/sites-available下
*	程序文件/usr/sbin/nginx
*	日志在/var/log/nginx目录中
*	启动脚本nginx在/etc/init.d/下
*	默认的虚拟主机的目录设置在了/var/www/nginx-default (有的版本默认的虚拟主机的目录设置在了/var/www, 请参考/etc/nginx/sites-available里的配置)

**在/etc/nginx目录下，nginx.conf是Nginx的核心配置文件，可以修改处理器数量、日志路径、pid文件等。在nginx.conf文件中有一段代码：`inxclude /etc/nginx/conf.d/*.conf`,这表示可以将自己的配置文件放在conf.d/中，Nginx会自动识别这个配置文件**
#### 3.修改配置文件
`vim /etc/nginx/sites-available/default`
```bash
server {
        listen 8080 default_server;
        listen [::]:8080 default_server;
        #root /usr/share/nginx/html;
        root /usr/local/data/file;
        server_name _;
        location / {
           # First attempt to serve request as file, then
           # as directory, then fall back to displaying a 404.
           try_files $uri $uri/ =404;
           autoindex on;#显示目录
           autoindex_exact_size on;#显示文件大小
           autoindex_localtime on;#显示文件时间
           charset utf-8;
        }
}
```
#### 4.启动nginx
```bash
/etc/init.d/nginx start
```
#### 5.浏览器访问
浏览器打开：`http://localhost:8080`可以看到/data/file目录下的文件，点击可下载

#### 6.修改配置后重载
```bash
/etc/init.d/nginx reload
```

**本文链接：[Nginx搭建HTTP文件服务器](http://www.ergzcode.com/2019/11/14/nginx-build-http-file-system.html)
欢迎转载，请注明出处！**