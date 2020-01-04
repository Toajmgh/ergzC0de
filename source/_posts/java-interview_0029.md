title: 面试官：Maven的jar包冲突如何解决
author: ergz
tags:
  - Java
  - 面试题
categories:
  - 搞定Java面试题
date: 2019-01-31 16:58:00
---
### 介绍
本文主要介绍了解决Maven中jar包冲突的几种方式

### 依赖传递
假设我们现在又一个多模块的项目，依赖关系如下，我们在st-web模块中引入st-dal依赖时，st-common-lib这个依赖也会被我们引入，这个就是依赖传递。下表列出了scope在依赖过程中发生的变化，列标题为被依赖的模块，每行为要依赖的模块

<!--more-->

**st-common-lib <- st-dal <- st-web**
 
[scope]| compile | test | provided | runtime
-|-|-|-|-
compile | compile | - | - | runtime
test | test | - | - | test
provided | provided | - | provided |provided
runtime | runtime | - | - | runtime

### 依赖仲裁
依赖仲裁就是当项目中引入的jar包，groupID（公司名倒过来）和artifactID（功能命令）一样，但是version不一样，应该选用哪一个version？经常也被人叫做依赖冲突

**最短路径原则**

假如说我们现在的项目依赖关系如下，那么maven会选用st-common-lib的那个版本呢？

答案是1.1这个版本，st-web到st-common-lib（1.1）的距离为1，st-web到st-common-lib（1.0）的距离为2，选用距离短的，即最短路径原则

**st-common-lib(1.0) <- st-dal <- st-web -> st-common-lib(1.1)**

如何看出依赖的距离关系呢？之前的一篇关于maven的文章说过，执行如下命令打印出全局的依赖树，层级关系特别清楚
`mvn dependency：tree > show.txt`

**声明优先原则**

项目依赖如下，路径一样？会选用st-common-lib的哪个版本呢？这就得看你在pom文件中先声明哪个依赖，如果在pom.xml文件中，st-remote-invoke写在前面，就会用1.0这个版本，如果st-dal写在前面，则会用1.1这个版本

**st-common-lib（1.0） <- st-remote-invoke <- st-web**<br/>
**st-common-lib（1.1） <- st-dal <- st-web**

**依赖排除**

去掉间接引入的jar包

如果不想用spring boot默认提供的log，想集成第三方的log时

或者说上面依赖仲裁的第二个例子中，只想用st-common-lib的1.1版本，就可以把1.0版本排除

	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter</artifactId>
		<exclusions>
			<exclusion>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-starter-logging</artifactId>
			</exclusion>
		</exclusions>
	</dependency>

**本文链接：[面试官：Maven的jar包冲突如何解决]()
欢迎转载，请注明出处！**