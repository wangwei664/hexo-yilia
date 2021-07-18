---
title: Git 学习笔记一
toc: true
date: 2017-07-14 10:10:02
tags: Git
categories: 开发工具
---

# Git是什么？
Git 是一款免费、开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理。Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。  

![图片无法正常显示](http://zhijiechi.com/xiaowang/git/0git_bash_logo.png)

<!-- more -->

# 什么是版本控制系统？  
类比写毕业论文时的场景简单说明一下，所谓版本就相当于毕业论文写作时的各种副本，管理起来非常麻烦，写过的人肯定都深有体会[很忧伤又很无奈有木有]......  
话不多说直接上图：（以我当年毕业论文为例）  

![图片无法正常显示](http://zhijiechi.com/xiaowang/git/version_management.png)

若使用版本管理工具，版本管理很轻松就能解决啦，而且还能版本间的自由切换。  
为什么要举这个例子，因为每次看到git，我脑海中立即就会浮现出当时写论文的画面，瞬间就会感慨：当时若会git该多好啊...  

# 安装Git  
git 在 Linux、Mac、Win 系统都可以安装。本文使用win7 x64系统。  

下载地址：[git-for-windows](https://git-for-windows.github.io "点我下载哦")  
下载后差不多一路Next正常安装即可  

因为Git是分布式版本控制系统，所以下载完成后，每个机器都必须自报家门：设置你的昵称和Email地址（玩游戏也需要先注册个账号嘛）  

	$ git config --global user.name "Your Name"  
	$ git config --global user.email "email@example.com"

否则后面执行commit命令时，会收到如下提示

	$ git commit -m 'create demo.txt'
	
	*** Please tell me who you are.
	
	Run
	
	  git config --global user.email "you@example.com"
	  git config --global user.name "Your Name"
	
	to set your account's default identity.
	Omit --global to set the identity only in this repository.
	
	fatal: empty ident name (for <(null)>) not allowed


# 快速体验  

## 代码管理  
> Git Bash 中的复制 `Ctrl + Insert`, 粘贴`Shift + Insert`

### 创建版本库 [本地仓库]  
打开Git，进入 Git Bash 中，执行以下命令

	$ cd D:/ #切换到D盘下
	$ mkdir gitlearn #创建目录gitlearn
	$ cd gitlearn #切换到gitlearn目录下 
	$ git init #初始化本地版本库

这时在 D:/gitlearn 目录下，会多出一个 .git 文件夹。若看不到，设置显示隐藏文件即可。

### 添加文件  
在 D:/gitlearn 目录下，使用自己喜欢的文本编辑器(Sublime/editplus, vim等)开发程序，这里为了方便就简单创建一个demo.txt文件来体验  

查看版本库状态  
`$ git status`  
此时 git 会提示有一个未被追踪的的文件demo.txt  
 
执行以下命令，让git仓库追踪管理demo.txt文件  

	$ git add demo.txt #把demo.txt文件添加到暂存区
	$ git status
	$ git commit -m "created demo.txt" #把demo.txt文件提交到版本库，并添加备注
	$ git status

> 特别提醒 : 一定记得多使用 `$ git status` 命令查看版本库状态，会有意想不到的收获哟 ：）

### 修改文件  
修改demo.txt文件，在第2行添加内容“I modified demo.txt”.  
文件被修改后，要重新提交到版本库，流程和添加文件时一样。  

执行以下命令，让git仓库记录此次修改文件操作

	$ git add demo.txt  
	$ git commit -m "modified demo.txt"


### 删除文件  
在 D:/gitlearn 目录下，新建一个wait_delete.txt文件，来体验删除文件操作  

	$ touch wait_delete.txt #创建wait_delete.txt文件
	$ ls #查看当前目录下的文件列表
	
	$ git add wait_delete.txt
	$ git commit -m "created wait_delete.txt"
	
	# 开始删除 删除文件后直接commit提交到版本库即可  
	$ git rm wait_delete.txt
	$ git commit -m "deleted wait_delete.txt"
	
	$ ls #再查看下当前目录下的文件列表
	
> 附：`$ git rm -r dirName` #表示 递归删除，多用于删除文件夹

查看操作日志  

	$ git log
	$ git reflog


## 远程仓库  
通过上面的“快速体验”->“代码管理”，我们对使用本地仓库管理代码应该是比较熟悉了。但如果是团队协同开发，就需要把版本仓库放到互联网上，开发者可以自由地把自己最新的代码版本推到远程仓库，或把远程仓库上的最新代码拉到自己本地，这样就可以实现协作开发了。  

### 注册 Git 在线仓库账号  
GitHub：[https://github.com](https://github.com)  
oschina：[https://git.oschina.net](https://git.oschina.net)  
推荐使用github

#### 注册 github 账号 [已有账号的直接使用即可]
> 什么？网站打不开？慢到无法访问？嗯有墙，建议先把git放一下，去掌握下怎么科学上网。  
> 如果你嫌麻烦，又想立即体验一把的话，那就使用oschina。

#### 创建版本库 [远程仓库]  
这里是不是有种似曾相识的感觉，对，上面体验过创建本地版本库嘛。  
我在远程的github账户中创建了一个名为 gitlearn 的仓库，最好和本地版本库名字保持一致。
  
github 为每个仓库提供了两个地址：  
HTTPS地址：https://github.com/itxiaowang/gitlearn.git  
SSH地址：git@github.com:itxiaowang/gitlearn.git  
这里为了快速体验，选择使用https地址  

#### 把代码推到远程仓库  
##### 为本地仓库添加一个对应的远程仓库  

	$ git remote add origin https://github.com/itxiaowang/gitlearn.git
	#添加一个远程版本库，简称为 origin，地址为 https://github.com/itxiaowang/gitlearn.git
	#可以执行 git remote 或 git remote -v 查看下当前存在的远程版本库

##### 执行push推送
	$ git push origin master #将本地代码版本（默认是master）推送到远程仓库
  
> 因为使用的HTTPS地址，所以每次推送时需要输入用户名和密码，即注册时的账号密码。  

输入账号密码后，报错如下：

	To https://github.com/itxiaowang/gitlearn.git
	 ! [rejected]        master -> master (fetch first)
	error: failed to push some refs to 'https://github.com/itxiaowang/gitlearn.git'
	hint: Updates were rejected because the remote contains work that you do
	hint: not have locally. This is usually caused by another repository pushing
	hint: to the same ref. You may want to first integrate the remote changes
	hint: (e.g., 'git pull ...') before pushing again.
	hint: See the 'Note about fast-forwards' in 'git push --help' for details.  
	
	#大概是说 更新被拒绝因为远程仓库中已经包含你所做的内容；
	#这通常是由另一个仓库push所致；
	#你应该在再次push前使用 git pull 集成远程更改

仔细阅读提示，就明白了失败原因和解决方法。  
&emsp;&emsp;原因：远程仓库中存在了本地仓库中不存在的提交，往往是多人协作开发过程中造成了版本冲突，先同步（git pull拉取合并）再git push推送即可。  
&emsp;&emsp;说明：假设一个简单开发场景来说明。git是分布式版本控制器，假若团队协同开发某一项目，早上刚上班小A，小B的本地仓库代码版本都和远程仓库完全一致。一会儿小A在自己本地仓库修改了某个内容并push到了远程仓库；接着小B也在自己的本地仓库修改了某个内容，当push到远程仓库时就会收到提示说版本库中已经包含一些内容，不允许直接覆盖。因为此时小A已经修改了远程仓库中的代码版本，小A本地仓库代码版本和远程仓库一致，小B则不再一致，假若此时小B可以直接push成功的话就会将小A修改内容给覆盖掉，所以push应该是被拒绝的。  
我这里是因为在远程仓库手动修改提交了一下REANME.md文件，导致了代码版本冲突。  
&emsp;&emsp;解决冲突方法：  
方法1：

	$ git push origin master -f  
	#强制推送覆盖掉远程仓库代码版本

> 特别提醒：此方法尽量不要使用，又可能造成不可预知的错误！我这里完全确定是新建仓库并且是自己在远程仓库端手动修改提交了一个README.md文件而已，并没有什么重要文件，确保安全才可以使用  

方法2：

	$ git pull origin master  
	# 将远程仓库的代码版本拉取下来并与自己本地仓库的代码版本合并  

采用方法2 又有报错如下：  

	$ git pull origin master
	From https://github.com/itxiaowang/gitlearn
	 * branch            master     -> FETCH_HEAD
	fatal: refusing to merge unrelated histories  
	# 拒绝合并不相关的历史版本

解决方法：
  
	git pull origin master --allow-unrelated-histories
	# 设置允许合并不相关的历史版本  

接着会出现如下提示，让输入合并备注 [需要了解简单的vim命令]

	Merge branch 'master' of https://github.com/itxiaowang/gitlearn
	
	# Please enter a commit message to explain why this merge is necessary,
	# especially if it merges an updated upstream into a topic branch.
	#
	# Lines starting with '#' will be ignored, and an empty message aborts
	# the commit.

输入备注

	# 进入编辑模式：输入字母 i  
	# 填写备注内容 [任意内容都行] In order to resolve the conflict.
	# 保存退出：点击 Esc 键进入命令行模式，依次输入  :  wq

	From https://github.com/itxiaowang/gitlearn
	 * branch            master     -> FETCH_HEAD
	Merge made by the 'recursive' strategy.
	 README.md | 1 +
	 1 file changed, 1 insertion(+)
	 create mode 100644 README.md
  
说明拉取合并成功，这时在我本地版本库中看到多了个README.md文件  

此时，再执行push推送操作  

	$ git push origin master
	Username for 'https://github.com': itxiaowang
	Counting objects: 15, done.
	Delta compression using up to 4 threads.
	Compressing objects: 100% (8/8), done.
	Writing objects: 100% (15/15), 1.36 KiB | 0 bytes/s, done.
	Total 15 (delta 1), reused 0 (delta 0)
	remote: Resolving deltas: 100% (1/1), done.
	To https://github.com/itxiaowang/gitlearn.git
	   c9d15e8..b5c3a1b  master -> master

附：参考文献：[Stack Overflow : Git refusing to merge unrelated histories](https://stackoverflow.com/questions/37937984/git-refusing-to-merge-unrelated-histories "解决方案参考")

##### 远程仓库推送测试  

查看当前远程仓库gitlearn的demo.txt文件，里面只有2行内容
> I am learning git.  
> I modified demo.txt.  

修改下demo.txt文件，再添加一行内容“remote repo push test”,并推送到远程仓库中  

	$ git add demo.txt  
	$ git commit -m "added content 'push test'"  
	$ git push origin master

此时再查看远程仓库gitlearn中的demo.txt文件，里面有3行内容  
> I am learning git.  
> I modified demo.txt.  
> remote repo push test  

说明远程推送已成功.体验到此结束。