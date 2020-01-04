title: 最新IDEA永久激活
author: ergz
tags:
  - IDEA
categories:
  - 工具
date: 2019-12-25 20:26:00
---
**已支持2019.2的版本且适用Windows、Mac、Ubuntu等所有平台**

### 1.下载新版破解补丁
下载链接：<http://cdn.ergzcode.com/jetbrains/jetbrains-agent.jar>，并将它放置到IDEA安装目录的bin目录下（位置可随意）。

### 2.修改配置文件
如果你是刚下载的IDEA，先点击激活窗口的免费试用，创建一个新项目来进入到IDEA的工作目录。进入IDEA的工作界面后，点击IDEA的上方的菜单栏‘Help’->‘Edit Custom VM Options...’，如果提示是否要创建文件，请点‘Yes’，打开idea64.exe.vmoptions文件在末行添加：-javaagent:你的IDEA安装目录\bin\jetbrains-agent.jar
<!--more-->

	-Xms512m
	-Xmx2048m
	-XX:ReservedCodeCacheSize=480m
	-XX:+UseConcMarkSweepGC
	-XX:SoftRefLRUPolicyMSPerMB=50
	-ea
	-XX:CICompilerCount=2
	-Dsun.io.useCanonPrefixCache=false
	-Djava.net.preferIPv4Stack=true
	-Djdk.http.auth.tunneling.disabledSchemes=""
	-XX:+HeapDumpOnOutOfMemoryError
	-XX:-OmitStackTraceInFastThrow
	-Djdk.attach.allowAttachSelf
	-Dkotlinx.coroutines.debug=off
	-Djdk.module.illegalAccess.silent=true
	-javaagent:C:\Program Files\JetBrains\ideaIU 2019.2.3\bin\jetbrains-agent.jar

请仔细检查补丁路径是否正确，如果错误则会出现IDEA打不开的情况，这时候可以删除用户配置目录下的IDEA文件夹:

- windwos：C:\Users\用户名\
- macos：~/Library/Preferences/
- ubuntu：~/.	

**修改完配置文件之后重启IDEA**

### 3.输入激活码

重启IDEA之后，点击菜单栏中的 ‘Help ’-> ‘Register...’，这里有两种激活方式：

一.选择最后一种License server激活方式，地址填入：http://jetbrains-license-server ,或者点击按钮：‘Discover Server’来自动填充地址，完成激活。

二.如果服务器激活方式无法激活，还可以选择Activation code方式激活，激活码下载:[激活码.txt](http://cdn.ergzcode.com/jetbrains/%E6%BF%80%E6%B4%BB%E7%A0%81.txt)，下载复制激活码填入，点击OK即可。


服务器激活是没有期限的，是永久有效。激活码激活的有效期到2089年。

**本文链接：[最新IDEA永久激活]()
欢迎转载，请注明出处！**