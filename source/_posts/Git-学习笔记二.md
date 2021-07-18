---
title: Git 学习笔记二
toc: true
date: 2017-07-14 12:02:10
tags: Git
categories: 开发工具
---

# Git 的特点和发展历史

## 特点：分布式版本控制器  
分布式与集中式的对比  

集中式 以 svn 为例：  
![图片无法正常显示](http://zhijiechi.com/xiaowang/git/1centralized.png "centralized")  

<!-- more -->

中心的svn服务器，统一存储着代码版本的变迁及日志.  
查看改动日志，回退代码版本，创建新的分支 都必须联网且连接到svn服务器.  
假如中心的svn服务器宕机，就意味着所有文件全部丢失.

分布式 以 git 为例：  
![图片无法正常显示](http://zhijiechi.com/xiaowang/git/2destributed.png "destributed")  

中心的git服务器，也是起到一个存储和交换最新版本的作用.  
每个开发者电脑上都有完整的代码版本、日志和分支信息.开发者不必依赖于服务器就可以查看日志、回退版本、创建分支.  
假如中心的git服务器宕机，开发者可以从任意一台保存有原有代码版本的电脑上重新获取并恢复.  

## 发展历史  
......

# 代码管理  

## 工作区、暂存区和版本库  

要想清晰地学习和理解git，必须了解3个区域：  
+ 工作区：开发者的工作目录.  
+ 暂存区：操作已被记录，但尚未提交到版本库的暂存区域.  
+ 版本库：存储变化日志和版本信息的区域.  

![图片无法正常显示](http://zhijiechi.com/xiaowang/git/3area.png "工作区 暂存区 版本库")  

使用 `$ git status` 查看下创建文件并将其提交到版本库的全过程  
> `$ git status` ? 咦 怎么感觉这么熟悉呢？对，没错，还记得在前面快速体验git操作时的特别提醒么：“记得多用 `$ git status` ,会有意想不到的收获呦”  

	$ touch text.txt #在本地工作区 创建test.txt文件  
	$ git status #查看状态，如下  

	On branch master
	Untracked files:
	  (use "git add <file>..." to include in what will be committed)
	
	        test.txt
	
	nothing added to commit but untracked files present (use "git add" to track)

> 在 master 分支下有一个未被追踪的文件 test.txt  
> 可使用 git add fileName 把它添加到待提交列表 [即暂存区]  

	$ git add test.txt #将test.txt文件添加到暂存区  
	$ git status #查看状态，如下  
	
	On branch master
	Changes to be committed:
	  (use "git reset HEAD <file>..." to unstage)
	
	        new file:   test.txt
  
> 存在可提交的修改  
> + 此时可以使用 `git reset HEAD text.txt` 将test.txt文件从暂存区删除  

	# git commit -m "提交备注" #提交到版本库  
	$ git commit -m "created test.txt" #将test.txt文件提交到版本库，会看到如下信息  
	
	[master ece6dd6] created test.txt
	 1 file changed, 0 insertions(+), 0 deletions(-)
	 create mode 100644 test.txt
	
	$ git status #查看状态，如下  
	
	On branch master
	nothing to commit, working tree clean

> 没有可被提交的东西了，目前工作区是干净的.  

## 文件操作  
### 添加文件  

	$ git add fileName #添加fileName文件到暂存区
	$ git add fileName1 fileName2 fileName3 #添加fileName1、fileName2到暂存区  
	$ git add *.txt #添加当前目录下的所有.txt文档到暂存区  
	$ git add . #添加当前目录的所有文件到暂存区
  
### 删除文件  
	
	$ git rm fileName  

### 移动或改名  

	$ git mv 源文件 新文件  
	
	eg：  
	移动：git mv ./dir1/config.php ./dir2/config.php  
	改名：git mv config.php conf.php
 
## 改动日志  
每个文件/目录发生的版本变法都可以使用 `git log` 命令进行追溯.

	$ git log #查看项目的日志  
	$ git log fileName #查看某文件日志
  
eg:
	
	commit 4a0fe54cf1f2ca3fda63201fa641e25ef4ea3a19
	Author: xiaowang <wangwei19930418@163.com>
	Date:   Thu Jun 29 18:20:52 2017 +0800
	
	    modified demo.txt
	
	commit b1cf6ddde9d1e31cedff8eaa84100e52ed8f8343
	Author: xiaowang <wangwei19930418@163.com>
	Date:   Thu Jun 29 17:45:24 2017 +0800
	
	    created demo.txt
 
若感觉有些缭乱，可让日志单行显示  

	$ git log --pretty=oneline
	
	4a0fe54cf1f2ca3fda63201fa641e25ef4ea3a19 modified demo.txt
	b1cf6ddde9d1e31cedff8eaa84100e52ed8f8343 created demo.txt


## 版本切换  
	$ git reflog #查看版本变化  
	
	[版本号]                  [提交备注]
	ece6dd6 HEAD@{0}: commit: created test.txt
	5651b92 HEAD@{1}: commit: added content 'push test'
 
方法1：使用 HEAD指针 来切换.  

	HEAD指向的当前版本为 ece6dd6  
	$ git reset --hard HEAD^ #切换到当前版本的前1版本  
	$ git reset --hard HEAD^^ #切换到当前版本的前2版本  
	$ git reset --hard HEAD^^^ #切换到当前版本的前3版本  
	$ git reset --hard HEAD~100 #切换到当前版本的前100版本
 
方法2：使用 版本号 来切换.  

	$ git reset --hard 5651b92
	HEAD is now at 5651b92 added content 'push test'  
	
	$ git reset --hard ece6dd6
	HEAD is now at ece6dd6 created test.txt
 
注意：版本号不需要写的过长，只要能保证唯一性即可.  
eg：`$ git reset --hard ece6`  

# 分支管理  

## 分支的用途  
类比生活场景：  
假设我在完成一篇毕业论文的过程中，已经写好了论文初稿，指导老师看后给出了修改意见说论文选题和整体架构还是不错的，但如果能在样本选择、资料收集方法、行文结构上做下调整会更好，我就要按照意见修改。  
这时假如我直接在初稿的源文件上修改，有可能导致一种结果：论文修改的并没有达到预想效果，但初稿也已经面目全非，此时我便会陷入进退两难的境地。  
若是我在修改前将初稿源文件复制一份，将源文件保存，在副本上进行修改，如果改的满意我可以使用副本改名后直接提交给指导老师审阅，若是感觉改的还不如初稿好想要返回，那我就直接放弃该文件，找到初稿源文件再重新复制一份进行修改即可。

模拟开发场景：  
假设目前手里有个小项目，要开发一个产品管理系统，开发周期设为7天吧。  
这时假如我直接在源码文件上进行的开发。  

## 查看分支  	
	$ git branch #查看所有分支  
eg:
	
	$ git branch
	* master
	#说明当前只有master分支，且处于master分支
 
## 创建分支  
	$ git branch branchName #创建某个分支
eg:
	
	$ git branch dev #创建dev分支  
	$ git branch
	  dev
	* master
	# 说明当前有 master 和 dev 两个分支，且处于master分支

## 切换分支  
	$ git checkout branchName #切换到某个分支上  
eg:

	$ git checkout dev #切换到dev分支
	Switched to branch 'dev'
	
	$ git branch
	* dev
	  master
	# 说明当前已经处于dev分支

## 合并分支  
	$ git merge branchName #合并分支  

假设当前我们在dev分支上开发某功能，并测试测试后，可以把dev分支上的内容合并到master分支上.  
eg：当前 test.txt 文件内容为空，在dev分支上，添加内容“contents from dev.”,并提交；然后切换到master分支上，查看下test.txt文件的内容仍为空；然后执行`git merge dev`合并下dev分支，在查看test.txt文件，内容就不再为空而是“contents from dev.”啦.

	$ git checkout dev #切换到dev分支上
	
	#在test.txt文件中添加内容“contents from dev.”
	$ git add test.txt
	$ git commit -m "merge dev branch test"
	#将添加操作提交到仓库
	
	$ git checkout master #切换到master分支上
	Switched to branch 'master'
	#此时查看下 test.txt 文件，发现内容仍然为空。
	
	$ git merge dev #合并dev分支
	Updating ece6dd6..fa96b9e
	Fast-forward
	 test.txt | 1 +
	 1 file changed, 1 insertion(+)  
	
	#此时，再查看下 test.txt 文件，就会发现dev分支上添加的内容“contents from dev.”了.

## 删除分支  
	git branch -d branchName #删除某分支  

	$ git branch -d dev #删除dev分支
	Deleted branch dev (was fa96b9e).
	
	$ git branch
	* master 
	# 只剩下master分支


## 快速创建并切换分支  
	$ git checkout -b branchName #创建并切换到某分支  
即是 `$ git branch branchName` 和 `$ git branch branchName` 的合并.


# 远程仓库  

## 查看远程仓库
	$ git remote #查看远程仓库  
	$ git remote -v #查看远程仓库地址  
eg:

	$ git remote
	origin
	
	$ git remote -v
	origin  https://github.com/itxiaowang/gitlearn.git (fetch)
	origin  https://github.com/itxiaowang/gitlearn.git (push)
 
## 添加远程仓库  
	$ git remote add <远程库名> <远程库地址>  
eg：  
	
	$ git remote add origin https://github.com/itxiaowang/gitlearn.git

## 修改远程仓库  
	$ git remote rename <旧名称> <新名称>  
eg:  

	$ git remote rename origin originhttps
	
	$ git remote -v
	originhttps     https://github.com/itxiaowang/gitlearn.git (fetch)
	originhttps     https://github.com/itxiaowang/gitlearn.git (push)

## 删除远程仓库  
	$ git remote remove <远程库名>  

## 推送分支到远程库  
推送分支，就是把该分支上的所有本地内容提交推送到远程库。  
推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：  
	
	$ git push origin master  

如果要推送其他分支，比如dev，就改成：  
	
	$ git push origin dev

### 配置SSH方式的远程库  
上面快速体验采用的https方式连接远程库，每次都要输入用户名和密码，有些麻烦，这里配置公钥、采用SSH方式连接远程库可以解决此问题。  

#### 配置SSH格式的远程仓库地址
  
	$ git remote add <远程仓库名> <远程仓库ssh地址>  
eg:  
	
	$ git remote add origin git@github.com:itxiaowang/gitlearn.git

	$ git remote
	origin
	originhttps
	
	$ git remote -v
	origin  git@github.com:itxiaowang/gitlearn.git (fetch)
	origin  git@github.com:itxiaowang/gitlearn.git (push)
	originhttps     https://github.com/itxiaowang/gitlearn.git (fetch)
	originhttps     https://github.com/itxiaowang/gitlearn.git (push)

#### 创建ssh key  
	$ ssh-keygen -t rsa -C "youremail@example.com"
  
把 email地址 换成你自己的邮件地址，然后连按四下回车键即可[不用输入密码].  

	$ ssh-keygen -t rsa -C "wangwei19930418@163.com"
	Generating public/private rsa key pair.
	Enter file in which to save the key (/c/Users/Administrator/.ssh/id_rsa):
	Enter passphrase (empty for no passphrase):
	Enter same passphrase again:
	Your identification has been saved in /c/Users/Administrator/.ssh/id_rsa.
	Your public key has been saved in /c/Users/Administrator/.ssh/id_rsa.pub.
	The key fingerprint is:
	SHA256:GhMe34w95T+sE6ozpZvfdqXHV3S3kReo4xcXtLWe/II wangwei19930418@163.com
	The key's randomart image is:
	+---[RSA 2048]----+
	|              o..|
	|             . o+|
	|      o     o  o+|
	|     . + = = .o+*|
	|      + S * o o=*|
	|       +  .o.= .+|
	|      .  o .E.=+o|
	|        +...o.oo+|
	|        +*..oo ..|
	+----[SHA256]-----+

完成后，可以在 ~/.ssh 目录下 [ 即 用户主目录下，如 C:\Users\Administrator ] 找到 id_rsa [私钥] 和 id_rsa.pub [公钥] 两个文件。  
公钥和私钥是成对的，能让分别持有的双方互相认识。

#### 把公钥复制粘贴到到github账户的设置中
  
![图片无法正常显示](http://zhijiechi.com/xiaowang/git/4ssh_keys_config.png "添加公钥到github账户的设置中")

#### 再push本地仓库的代码到远程仓库时，就会发现不需要每次都输入用户名密码了.  
	$ git push origin master  

执行推送命令后回车，会出现以下消息

	The authenticity of host 'github.com (192.30.253.113)' can't be established.
	RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
	Are you sure you want to continue connecting (yes/no)? yes
  
输入`yes`,回车即可

	Warning: Permanently added 'github.com,192.30.253.113' (RSA) to the list of known hosts.
	Counting objects: 6, done.
	Delta compression using up to 4 threads.
	Compressing objects: 100% (4/4), done.
	Writing objects: 100% (6/6), 529 bytes | 0 bytes/s, done.
	Total 6 (delta 1), reused 0 (delta 0)
	remote: Resolving deltas: 100% (1/1), done.
	To github.com:itxiaowang/gitlearn.git
	   5651b92..fa96b9e  master -> master
 
说明 分支推送已完成。

# 克隆远程仓库  
	git clone <远程仓库地址> <本地存放目录>  
eg：将远程gitlearn仓库中的内容 克隆到 本地 D:/gitlearn2 目录

	$ cd D:/ #切换到D盘目录下
	$ git clone git@github.com:itxiaowang/gitlearn.git ./gitlearn2 #克隆远程仓库到本地
	
	Cloning into './gitlearn2'...
	remote: Counting objects: 29, done.
	remote: Compressing objects: 100% (15/15), done.
	remote: Total 29 (delta 3), reused 22 (delta 2), pack-reused 0
	Receiving objects: 100% (29/29), done.
	Resolving deltas: 100% (3/3), done.
 
此时，查看D盘下，多了个gitlearn2目录，里面文件及内容和远程仓库gitlearn中的一样。  

## 说明
+ 前面快速体验时，我先创建了本地库，后创建了远程库。下面简述下如何关联远程库：  

现在，假设我们从零开发，最好的方式是先创建远程库，然后从远程库进行克隆。  

首先，登陆GitHub，创建一个新的仓库，名字叫gitskills：

我们勾选Initialize this repository with a README，这样GitHub会自动为我们创建一个README.md文件。创建完毕后，可以看到README.md文件：  

现在，远程库已经准备好了，下一步是用命令git clone克隆一个本地库：  

注意把Git远程库地址换成你自己的，然后进入gitskills目录看看，已经有README.md文件了。  

如果有多个人协作开发，那么每个人各自从远程库中克隆一份到自己本地就可以了。  

注：GitHub远程库一般给出两个地址：ssh地址 和 https地址。Git支持多种协议，默认的git://使用ssh，但也可以使用https等其他协议。使用https除了速度慢以外，还有个最大的问题是每次推送都必须输入口令用户名和密码。  


# 参考文献  

[Git Manual](https://git-scm.com/docs "Git官方手册")  
[Git Cheat Sheet](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf "Git小抄")  
[Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013760174128707b935b0be6fc4fc6ace66c4f15618f8d000 "廖雪峰网站Git教程")