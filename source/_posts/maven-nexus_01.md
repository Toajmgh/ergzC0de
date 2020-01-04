title: Maven之私服nexus
author: ergz
tags: []
categories:
  - 工具
date: 2016-05-24 19:03:00
---
### 介绍
Maven仓库分为本地仓库，远程仓库（私服），中央仓库，本文主要介绍了私服的使用。

### 将项目发布到私服
首先要把代码上传到私服上，必须要能登录到私服上，在Maven的settings.xml文件，配置连接私服的用户和密码。  
<!--more-->
1.在Maven配置文件settings.xml的<servers></servers>域中添加以下代码：
	
	<server>
		<id>releases</id>
		<username>admin</username>
		<password>admin123</password>
	</server>
	<server>
		<id>snapshots</id>
		<username>admin</username>
		<password>admin123</password>
	</server>

releases 连接发布版本项目仓库  
snapshots 连接测试版本项目仓库

2.配置项目pom.xml  
配置私服仓库的地址，本公司的自己的jar包会上传到私服的宿主仓库，根据工程的版本号决定上传到哪个宿主仓库，如果版本为release则上传到私服的release仓库，如果版本为snapshot则上传到私服的snapshot仓库  

	<distributionManagement>
        <repository>
            <id>releases</id>
            <url>http://localhost:8081/nexus/content/repositories/releases/</url>
        </repository>
        <snapshotRepository>
            <id>snapshots</id>
            <url>http://localhost:8081/nexus/content/repositories/snapshots/</url>
        </snapshotRepository>
    </distributionManagement>

**注意：pom.xml这里<id>和settings.xml配置<id>对应！**

### 从私服下载jar包
1.在setting.xml中配置仓库  
	
	<!-- 下载jar包配置 -->
	<profile> 
		<!--profile的id -->
		<id>dev</id>
		<repositories>
			<repository> 
					<!--仓库id，repositories可以配置多个仓库，保证id不重复 -->
				<id>nexus</id>
					<!--仓库地址，即nexus仓库组的地址 -->
				<url>http://localhost:8081/nexus/content/groups/public/</url>
					<!--是否下载releases构件 -->
				<releases>
					<enabled>true</enabled>
				</releases>
					<!--是否下载snapshots构件 -->
				<snapshots>
					<enabled>true</enabled>
				</snapshots>
			</repository>
		</repositories>
		<pluginRepositories>
			<!-- 插件仓库，maven的运行依赖插件，也需要从私服下载插件 -->
			<pluginRepository> 
				<!-- 插件仓库的id不允许重复，如果重复后边配置会覆盖前边 -->
				<id>public</id>
				<name>Public Repositories</name>
				<url>http://localhost:8081/nexus/content/groups/public/</url>
			</pluginRepository>
		</pluginRepositories>
	</profile>

使用profile定义仓库需要激活才可生效  

	<activeProfiles>
		<activeProfile>dev</activeProfile>
	</activeProfiles>

这里的activeProfile必须与上方profile的id一致


### 安装第三方jar包到本地仓库
这里将fastjson-1.1.37.jar上传到本地仓库，有2种方式：  
1.进入jar包所在目录运行

	mvn install:install-file -DgroupId=com.alibaba -DartifactId=fastjson -Dversion=1.1.37 
		-Dfile=fastjson-1.1.37.jar -Dpackaging=jar

2.打开cmd直接运行
	
	mvn install:install-file -DgroupId=com.alibaba -DartifactId=fastjson -Dversion=1.1.37 
		-Dpackaging=jar -Dfile=C:\Users\TOAJMER\Desktop\fastjson-1.1.37.jar


### 安装第三方jar包到私服
这里将fastjson-1.1.37.jar上传到私服，在settings配置文件中添加登录私服第三方登录信息：

	<server>
		<id>thirdparty</id>
		<username>admin</username>
		<password>admin123</password>
	</server>

有2种方式：  
1.进入jar包所在目录运行

	mvn deploy:deploy-file -DgroupId=com.alibaba -DartifactId=fastjson -Dversion=1.1.37 
		-Dpackaging=jar -Dfile=fastjson-1.1.37.jar 
		-Durl=http://localhost:8081/nexus/content/repositories/thirdparty/ 
		-DrepositoryId=thirdparty

2.打开cmd直接运行  

	mvn deploy:deploy-file -DgroupId=com.alibaba -DartifactId=fastjson -Dversion=1.1.37 
		-Dpackaging=jar -Dfile=C:\Users\TOAJMER\Desktop\fastjson-1.1.37.jar 
		-Durl=http://localhost:8081/nexus/content/repositories/thirdparty/ 
		-DrepositoryId=thirdparty

**本文链接：[Maven之私服nexus]()
欢迎转载，请注明出处！**
