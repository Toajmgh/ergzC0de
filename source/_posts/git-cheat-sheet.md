title: GIT命令清单
author: ergz
tags:
  - git
categories:
  - git
date: 2019-11-25 01:20:00
---
文档来源于[Tower](http://www.git-tower.com/)>Git版本控制系统-使一切变的简单

英文原版下载地址：<http://cdn.ergzcode.com/git/git-cheatsheet.pdf>

由ergzC0de翻译，如有错误之处，欢迎留言指正

----------
### 1.创建库

克隆一个现有版本库
	
	$ git clone ssh://user@domain.com/repo.git

<!--more-->

创建一个本地版本库

	$ git init	

### 2.本地更改

查看工作区的状态
		
	$ git status	

查看被跟踪的文件更改了什么内容
	
	$ git diff <file>
	
添加所有的文件修改到暂存区

	$ git add .	

指定某个文件添加到暂存区，-p参数可省略
	
	$ git add -p <file>	

提交跟踪文件中的所有本地更改，建议一般不使用-a参数

	$ git commit -a

将暂存区里的更改给提交到本地版本库，-m参数指定提交信息

	$ git commit -m "message"

更改最后一次提交,不修改已发布的提交,也就是追加提交，不会增加新的commit id将新的修改追加到前一次的commit id中

	$ git commit --amend

### 3.提交历史

显示提交的日志

	$ git log

某个文件的提交日志

	$ git log -p <file>

何人何时修改的某个文件日志
	
	$ git blame <file>

### 4.分支和标签

列出所有的分支

	$ git branch -av 

切换当前分支

	$ git checkout <branch>

创建新分支

	$ git branch <new-branch>

基于远程分支创建一个新的跟踪分支
	
	$ git checkout --track <remote/branch>

删除一个分支

	$ git branch -d <branch>

给当前提交贴一个标签，常用于发布版本
	
	$ git tag <tag-name>

### 5.更新和发布

列出所有当前配置的远程地址
	
	$ git remote -v

显示某个远程地址的信息

	$ git remote show <remote>

关联一个远程库，一般别名用origin

	$ git remote add <shortname> <url>

从远程库下载所有的更改，但不合并到当前分支上

	$ git fetch <remote>

从远程库下载所有的更改并直接合并到当前分支上

	$ git pull <remote> <branch>

推送本地修改到远程库

	$ git push <remote> <branch>

删除远程库的分支

	$ git branch -dr <remote/branch>

推送标签
	
	$ git push --tags

### 6.分支和重定

合并指定分支到当前分支上

	$ git merge <branch>
	
将最新的分支提交到本地	

	$ git rebase <branch>

中止一个rebase

	$ git rebase --abort
	
解决冲突后继续重新执行rebase	

	$ git rebase --continue

使用您配置的合并工具来解决冲突

	$ git mergetool

使用您的编辑器手动解决冲突和(解决后)标记文件作为解决
	
	$ git add <resolved-file>
	$ git rm <resolved-file>

### 7.撤销（回退）

撤销工作区的所有本地更改
	
	$ git reset --hard HEAD

丢弃工作区的修改，文件修改还没有添加到暂存区,推荐第2种
	
	$ git checkout HEAD <file>
	  or
	$ git checkout -- <file>


还原提交(通过产生具有相反更改的新提交)

	$ git revert <commit>

回退到某个版本

	//回退到上个版本
	$ git reset --hard HEAD^

	//根据指定commit id回退版本
	$ git reset --hard <commit>

保存未添加暂存区的所有更改

	$ git reset <commit>

保存未提交的本地更改

	$ git reset --keep <commit>

把添加到暂存区的内容撤销（补充）
	
	$ git reset HEAD <file>


**本文链接：[GIT命令清单]()
欢迎转载，请注明出处！**