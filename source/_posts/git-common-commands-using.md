title: Git常用命令使用
tags:
  - git
categories:
  - git
date: 2019-11-26 19:26:00
---
1.查看工作区的状态：`git status`

2.查看文件具体修改了什么内容：`git diff file`

3.查看提交的历史记录，该命令显示从最近到最远的历史记录：`git log`，如果嫌输出信息太多，可以试试加上`--pretty=oneline`参数：`git log --graph --pretty=oneline --abbrev-commit`，命令如下：
```bash
git log --graph --pretty=oneline --abbrev-commit
eede197ffb267fbda60482da9554ba654ff523e3 init hexo repo
1f2508638115730cb301f73c51b9971fca5c7f09 Initial commit
```
上面输出的一大串数字其实是 commit id(版本号)，这是一个SHA1计算出来的一个非常大的数字，用十六进制表示
<!-- more -->
4.`git reset`该命令用来回退到某个版本，在git中用`HEAD`表示当前版本，`HEAD^`表示上个版本，`HEAD^^`表示上上版本......
以此类推，`HEAD~100`表示往上100个版本
现在我们用命令回退到上个版本：
```bash
git reset --hard HEAD^
HEAD is now at 1f2508
```
可以根据版本号`commit id`指定回退到某个版本：
`git reset --hard eede19`
这里的版本号没必要写全，前几位就可以了，Git会自动去找，这样我们就又回到最新的版本了。Git版本的回退速度非常快，在Git内部有个指向当前版本的指针`HEAD`，所以我们在进行版本回退或回到最新的版本的时候，Git只是帮我们将`HEAD`指针指向某个版本号。

5.`git reflog`记录你的每一条命令，可以查看`commit id`,提交具体的操作和提交输入的信息，命令如下：
```bash
root@instance-my0hsclo:/usr/local/ergzC0de# git reflog 
eede197 HEAD@{0}: rebase finished: returning to refs/heads/master
eede197 HEAD@{1}: pull --rebase origin master: init hexo repo
1f25086 HEAD@{2}: pull --rebase origin master: checkout 1f2508638115730cb301f73c51b9971fca5c7f09
fe14566 HEAD@{3}: commit (initial): init hexo repo
```

6.`git checkout -- file`丢弃工作区的修改，文件修改还没有添加到暂存区，命令如下：
```bash
git checkout -- readme.txt
```
当文件修改后已经添加到了暂存区又作了修改，现在撤销修改就回到添加到暂存区后的状态。

7.`git reset HEAD file`把添加到暂存区的内容撤销，`HEAD`表示最新的版本
```bash
root@instance-my0hsclo:/usr/local/learngit# git reset HEAD readme.txt
Unstaged changes after reset:
M	readme.txt
```
8.查看分支：`git branch `

9.创建分支：`git branch <name>`
我们创建新分支dev时，Git会新建一个指针叫`dev`，指向当前分支`master`的相同的提交点（HEAD指针指向的是当前分支），再把`HEAD`指向`dev`，之后的操作就在`dev`分支上完成，如图：

![upload successful](/images/git_201912101316.png)

10.切换分支:`git checkout <name>`或者`git switch <name>`
创建并切换分支`git checkout -b <name>`或者`git switch -c <name>`

11.合并指定分支到当前分支上：`git merge <name>`
当`dev`分支开发结束后，我们需要把`dev`分支合并到`master`（主分支）上，Git是如何操作的呢？Git直接把`master`指向`dev`的当前提交，就完成了合并，这些操作都是靠指针来完成，文件并没有发生变化，如图：

![upload successful](/images/git_201912101318.png)
分支合并时，Git默认使用`Fast forward`模式，使用这种模式，删除分支后，会丢掉分支的历史信息，使用`--no-ff`可以禁用`Fast forward`模式 `git merge --no-ff -m "merge with no-ff" dev`，这样每次合并会创建一个新的commit，加上`-m`参数，写上commit的描述。

12.删除分支:`git branch -d <name>`
合并完分支后就可以删除`dev`分支，，删除`dev`分支就是把`dev`指针给删掉，删掉后我们就剩下了一条`master`分支，如图：

![upload successful](/images/201912101319.png)

13.查看分支合并图:`git log --graph`

团队协作的分支开发流程图：

![upload successful](/images/git_201912101326.png)

**本文链接：[Git常用命令使用](http://www.ergzcode.com/2019/11/26/git-common-commands-using.html)
欢迎转载，请注明出处！
参考：<https://www.liaoxuefeng.com/wiki/896043488029600>**