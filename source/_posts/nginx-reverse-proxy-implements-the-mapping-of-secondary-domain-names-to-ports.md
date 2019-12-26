title: Nginx反向代理实现二级域名与端口的映射
author: ergz
tags:
  - Nginx
categories:
  - linux
date: 2019-12-14 17:27:00
---
**在之前的一篇文章：[Nginx搭建HTTP文件服务器](http://www.ergzcode.com/2019/11/14/nginx-build-http-file-system.html)中,我们搭建了一个HTTP的文件服务器，通过8080端口来访问服务。现在我们想通过域名的方式（二级域名）来访问这个HTTP的文件服务器，我们知道通过浏览器URL地址的方式访问一个网站，比如`http://www.ergzcode.com`,默认的是去服务器找80端口下的服务，也就是`http://www.ergzcode.com:80`这个地址。考虑到80端口是比较特殊的端口，所以部署hexo时指定的端口是4000，这样将会导致只有使用`http://www.ergzcode.com:4000`才能访问到本站，但这种的URL无疑是让人接受不了的。**
<!-- more -->
**现在需要做到以下两点：**
*	将`http://www.ergzcode.com`的请求转发给4000端口下的应用程序，也就是通过这种URL访问本站
*	将`http://cdn.ergzcode.com`的请求转发给8080端口下的http文件服务器

**当然在这之前，有个前提就是你需要让`cdn.ergzcode.com`和`www.ergzcode.com`这两个域名解析到你的服务器IP上，DNS解析在域名提供商那里作处理,这里不再介绍**

**现在要做到上面的请求转发，就要用到Nginx的反向代理机制，下面来创建Nginx的配置文件，在`/etc/nginx/conf.d`目录下创建一个叫做reverse_proxy.conf的文件，文件名可以随意指定，但要以.conf为后缀名，然后加入下面的内容**
```bash
server
{
        listen 80;#监听端口
        server_name www.ergzcode.com;#根据域名跳转
        location / {
                proxy_redirect off;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_pass http://127.0.0.1:4000;#跳转的路径
        }
}
server
{
        listen 80;
        server_name cdn.ergzcode.com;
        #root /usr/local/data/file;
        location / {
                # try_files $uri $uri/ =404;
                proxy_redirect off;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_pass http://127.0.0.1:8080;#跳转的路径
        }
}

```
**执行命令使修改配置生效`/etc/init.d/nginx reload`**

访问本站：<http://www.ergzcode.com>
访问HTTP服务器：<http://cdn.ergzcode.com>
**完成！！**

**本文链接：[Nginx反向代理实现二级域名与端口的映射](http://www.ergzcode.com/2019/12/14/nginx-reverse-proxy-implements-the-mapping-of-secondary-domain-names-to-ports.html)
欢迎转载，请注明出处！**


