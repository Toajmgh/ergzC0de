title: JSP中basePath作用
author: ergz
tags:
  - Jsp
categories:
  - 前端
  - ''
date: 2015-05-19 16:11:00
---
通常在JSP页面中有如下代码：

```Java
<%  
String path = request.getContextPath();  
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";  
%>  
```

这段代码的意思是获取当前项目的路径，如：http://localhost:8080/项目名称。

在<head></head>中通常有如下代码：
```Java
<base href="<%=basePath%>">  
```

这是设置基础路径的，`basePath`为变量，简单的静态网页的话你设置比如：`<base href="http://www.baidu.com">`，那你下面的href属性就会以你上面设置的值的为基准，如：`<a href="http://www.baidu.com/xxx.htm"></a>`你现在就只需要写`<a href="xxx.htm"></a>`


**原文链接：<http://lzc0088.iteye.com/blog/504015>
欢迎转载，请注明出处！**