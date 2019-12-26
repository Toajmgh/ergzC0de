title: PM2介绍
tags:
  - pm2
categories:
  - linux
author: ergz
date: 2019-12-10 11:22:00
---
pm2 是一个带有负载均衡功能的Node应用的进程管理器。当你要把你的独立代码利用全部的服务器上的所有CPU，并保证进程永远都活着，0秒的重载， PM2是完美的。

主要特性：
1.内建负载均衡（使用Node cluster 集群模块）
2.后台运行
3.0秒停机重载，我理解大概意思是维护升级的时候不需要停机
4.具有Ubuntu和CentOS的启动脚本
5.停止不稳定的进程（避免无限循环）
6.控制台检测
7.提供 HTTP API
8.远程控制和实时的接口API(Nodejs模块,允许和PM2进程管理器交互 )
<!-- more -->
##### 安装

``` bash
npm install -g pm2 #命令行安装pm2
```
##### 用法

``` bash
pm2 start app.js -i 4 #后台运行pm2，启动4个app.js
pm2 start app.js --name my-api  #命名进程
pm2 list  #显示所有进程状态
pm2 monit  #监视所有进程
pm2 logs  #显示所有进程日志
pm2 stop all  #停止所有进程
pm2 restart all  #重启所有进程
pm2 reload all  #0秒停机重载进程 (用于 NETWORKED 进程)
pm2 stop 0  #停止指定的进程
pm2 restart 0  #重启指定的进程
pm2 startup  #产生 init 脚本 保持进程活着
pm2 web  #运行健壮的 computer API endpoint
pm2 delete 0  #杀死指定的进程
pm2 delete all  #杀死全部进程

运行进程的不同方式：
pm2 start app.js -i max  #根据有效CPU数目启动最大进程数目
pm2 start app.js -i 3  #启动3个进程
pm2 start app.js -x  #用fork模式启动 app.js 而不是使用 cluster
pm2 start app.js -x -- -a 23  #用fork模式启动 app.js 并且传递参数 (-a 23)
pm2 start app.js --name serverone #启动一个进程并把它命名为 serverone
pm2 stop serverone  #停止serverone进程
pm2 start app.json  #启动进程, 在app.json里设置选项
pm2 start app.js -i max -- -a 23  #在--之后给app.js传递参数
pm2 start app.js -i max -e err.log -o out.log  #启动 并生成一个配置文件
你也可以执行用其他语言编写的app ( fork 模式):
pm2 start my-bash-script.sh -x --interpreter bash
pm2 start my-python-script.py -x --interpreter python
pm2 list #列出由pm2管理的所有进程信息，还会显示一个进程会被启动多少次
pm2 monit #监视每个node进程的CPU和内存的使用情况。
```

**本文链接：[PM2介绍](http://www.ergzcode.com/2019/12/10/pm2-common-commands-introduction.html)
欢迎转载，请注明出处！**