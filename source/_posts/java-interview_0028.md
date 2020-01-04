title: 面试官：mvn package和mvn install都能打包，他们有区别吗
author: ergz
tags:
  - Java
  - 面试题
categories:
  - 搞定Java面试题
date: 2019-01-30 16:50:00
---
### 介绍
我们用maven打包一般用如下2个命令

mvn clean package 清理打包  
mvn clean install 清理打包，并将jar包或者war包复制到本地仓库

区别就是install比package多了一步将打好的jar包放到本地仓库的过程

我从头说一下maven帮我们做了那些，并解释一下maven的工程流程

<!--more-->

### 没有Maven之前的日子

个人的一个小感受，学习一个新技术，应该以历史的眼光来看待这个新技术出现的原因，以及帮我们解决了什么问题。我们来回忆一下没有Maven的日子是怎么样的？  

开发一个项目，需要用别人写好的jar包，我们先把开源的jar包下载下来放到项目的lib目录下，并且把这个目录添加到classpath（告诉Java执行环境，在哪些目录下可以找到你要执行的Java程序需要的类或者包）。我们下载了a.jar发现a.jar还需要依赖b.jar，结果又去把b.jar下载下来开始运行。如果运气够好，我们项目在添加完所有的依赖后，能正常运行了。如果运气不好，还会遇到jar包版本的问题，例如a.jar在调用b.jar的时候发现b.jar根本没有这个方法，在别的版本中才有，现在好了，光找依赖和适配版本就能花上不少时间。而且我们往git上上传代码的时候，还必须把这些lib都上传上去。别人下载我们的代码也必须把lib下载下来，这个真心耗费时间  

这时候Maven作为Java世界包管理工具出现了，当然Java的世界还有其他的包管理工具，例如gradle等。就像yum是Linux世界的包管理工具，webpack是前端世界的包管理工具一样

### Maven仓库的种类
**本地仓库，私服，中央仓库**  
Maven找jar包的过程是这样的，先在本地仓库找，找不到再去私服(如果配置了的话)，再找不到去中央仓库（Maven团队维护中）  

从中央仓库找到后，会在私服和本地仓库放一份，从私服找到后也会在本地仓库放一份  

当你安装好了Maven后，在conf目录下有个settings.xml文件，这个里面的配置的项很多，后文会详细介绍这个配置文件

	<!-- localRepository -->
	Default: ${user.home}/.m2/repository
	<localRepository>/path/to/local/repo</localRepository>

在这个配置文件下有这样的一段话，说了Maven默认的本地仓库的地址为${user.home}/.m2/repository（当然你可以重新设置本地仓库的地址，上面就是模板），我是windows电脑，来看看这个目录可以看到很多jar包被存在本地，当然如果你想配置私服也是在settings.xml进行配置，随便一搜很多教程，不再赘述  

搭建私服好处多多，在一个公司内部可以开发一些公共的基础组件放到私服上，方便其他同事使用

### Maven的默认配置
一个Maven的项目整体结构是这样的

在这里插入图片描述（图片省略...）

为什么一个Maven项目的文件结构是这种的呢？

这里就不得不说Maven的一个特性，约定优于配置

Maven默认配置了：  
${project.basedir}/src/main/java为项目的源代码目录  
${project.basedir}/src/main/test为项目的测试源代码目录  
${project.basedir}/target为项目的编译输出目录等   

spring boot 就是约定优于配置的体现，想想我们用springMVC的时候还得配置视图解析器，包的自动扫描，而用spring boot框架后，我们就完全不用再配置了

### Maven的项目详解
安装还是挺简单的，我就不再介绍了，我也没有单独下载，一般就有了idea自带的Maven插件，下载完后目录结构如下：  
**bin目录**
该目录包含了mvn运行的脚本，这些脚本用来配置java命令，准备好classpath和相关的Java系统属性，然后执行Java命令。  

**boot目录**
该目录只包含一个文件，该文件为**plexus-classworlds-2.5.2.jar**。plexus-classworlds是一个类加载器框架，相对于默认的Java类加载器，它提供了更加丰富的语法以方便配置，Maven使用该框架加载自己的类库  

**conf目录**
该目录包含了一个非常重要的文件settings.xml文件。直接修改该文件，就能在机器上全局的定制Maven的行为，即对所有用户都生效。一般情况下，我们更偏向于复制该文件至~/.m2/目录下（~表示用户家目录，windows下~就是C:\Users\{用户名}），然后修改该文件，在用户级别定制Maven的行为。  

**lib目录**
该目录包含了所有Maven运行需要的Java类库，Maven本身是分模块开发的，因此用户能看到诸如maven-core-3.0.jar，maven-model-3.0.jar之类的文件，此外这里还包含一些Maven用到的第三方依赖如common-cli-1.2.jar,common-lang-2.6.jar等等。

**settings.xml配置文件详解**
我们来细说一下settings.xml文件，这个文件可以定制Maven的行为，上面已经说到settings.xml可以可以放在2个位置，~/.m2/settings.xml(默认没有，需要我们自己复制)和${maven.home}/conf/settings.xml

这2个配置文件的加载顺序为~/.m2/settings.xml > ${maven.home}/conf/settings.xml，为了不影响他人，所以我们将conf下的settings.xml配置文件复制到家目录，在用户级别定制Maven的行为

这个和配置环境变量有点类似，Windows和Linux都可以配置系统级别的环境变量和用户级别的环境变量，这里单说一下Linux的吧，在/etc/profile里面配置的就是系统级别的环境变量，在~/.bash_profile里面配置的就是用户级别的环境变量

各种配置项还是挺多的，设置镜像仓库（国内用阿里云的比较多），设置代理，不再赘述

### Maven常用命令
命令 | 描述
-|-
mvn -version | 显示版本信息|
mvn clean | 删除target目录|
mvn compile | 编译src/main/java下的代码|
mvn package | 打包，在target目录下生成jar或war包|
mvn test | 执行src/test/java下以Test开头或者以Test结尾的的测试用例|
mvn install | 打包，并把jar或war包复制到本地仓库，供其他模块引用|
mvn deploy | 将打包的文件发到私服|
mvn dependency：tree | 打印出项目整个依赖树|

当然也可以连着使用  
mvn clean package 清理打包  
mvn clean package -DskipTest=true 清理打包，并跳过测试用例
mvn clean install 清理打包，并将jar包或者war包复制到本地仓库

运行单元测试的时候也没必要一个一个的点击测试方法，mvn test一个命令跑完所有的测试用例，要注意的是只会执行以Test开头或者结尾的测试类，也没必要自己写测试类，网上有些文章介绍了快速生成测试类的方法，可以去搜索看看，生成的测试类都是以Test开头或结尾的，这里不再赘述。

mvn dependency:tree > show.txt将项目的整个依赖重定向到文件中，方便查看  

**本文链接：[面试官：mvn package和mvn install都能打包，他们有区别吗]()
欢迎转载，请注明出处！**





