title: PM2守护hexo的进程
s: Test Hexo Writing Title
tags:
  - pm2
  - hexo
categories:
  - shell
author: ergz
date: 2019-12-08 08:01:00
---
在服务器上安装好了hexo，执行hexo sever来启动它的服务，会发现过了一段时间hexo的进程总是莫名其妙的挂掉，然后导致网站无法访问。从网上搜索了一些解决方案，发现能使用PM2来接管hexo的后台进程，从而让hexo进程一直常驻后台。

操作步骤如下：
##### 1.首先安装pm2
``` bash
npm install -g pm2
```
<!-- more -->
##### 2.写一个shell hexo_daemon.js
``` bash
//run
const { exec } = require('child_process')
exec('hexo server -p 80 & ',(error, stdout, stderr) => {
        if(error){
                console.log('exec error: ${error}')
                return
        }
        console.log('stdout: ${stdout}');
        console.log('stderr: ${stderr}');
})

```
##### 3.在根目录下执行shell

``` bash
pm2 start hexo_daemon.js
```
**本文链接：[PM2守护hexo的进程]()
欢迎转载，请注明出处！
参考:[PM2使用](https://www.jianshu.com/p/4a3d4d144ab2)**