---
layout: post
title: "git"
date: 2017-11-20 22:11:57
comments: ture
categories:
	- git
tags:
	- github
---

![](https://github.com/aqqje/images/raw/master/images/git.jpg "git")<br/>

- Git是什么？

- Git是目前世界上最先进的分布式版本控制系统（没有之一）。

- Git有什么特点？简单来说就是：高端大气上档次 !@#!@#%!@# 什么的，你懂的哈 就是贼厉害！
<!-- more -->
## Git的诞生
同生活中的许多伟大事件一样，Git 诞生于一个极富纷争大举创新的年代。Linux 内核开源项目有着为数众广的参与者。绝大多数的 Linux 内核维护工作都花在了提交补丁和保存归档的繁琐事务上（1991－2002年间）。到 2002 年，整个项目组开始启用分布式版本控制系统 BitKeeper 来管理和维护代码。

到了 2005 年，开发 BitKeeper 的商业公司同 Linux 内核开源社区的合作关系结束，他们收回了免费使用 BitKeeper 的权力。这就迫使 Linux 开源社区（特别是 Linux 的缔造者 Linus Torvalds ）不得不吸取教训，只有开发一套属于自己的版本控制系统才不至于重蹈覆辙。他们对新的系统制订了若干目标：

- 速度
- 简单的设计
- 对非线性开发模式的强力支持（允许上千个并行开发的分支）
- 完全分布式
- 有能力高效管理类似 Linux 内核一样的超大规模项目（速度和数据量）<br/>

自诞生于 2005 年以来，Git 日臻成熟完善，在高度易用的同时，仍然保留着初期设定的目标。它的速度飞快，极其适合管理大项目，它还有着令人难以置信的非线性分支管理系统。

## 安装Git(只介绍winn7系统)

- [git下载官网](https://git-scm.com/downloads)
- 一路next安装即可
- 安装完成，在开始菜单里找到“Git”->“Git Bash”，出现类似命令行窗口的东西，即Git安装成功！
- 安装完成后，还需要最后一步设置，在命令行输入：
- 

	git config --global user.name "Your Name"（Your Name是你github上的用户名，如：aqqje）
	git config --global user.email "email@example.com"

## 创建版本库

说明：

什么是版本库呢？版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”


- 创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：


		mkdir learngit       -- 新建一个名叫learngit的文件夹
		cd learngit          -- 进入learngit文件夹
	    pwd                  -- 显示当前目录


- 初始化仓库


		git init 			   -- 初始化learngi文件夹为仓库


- 初始化仓库后会出现一个 .git 目录(默认隐藏)，使用ls -ah命令就可以看见。


## git指令

说明:

在讲解git指令之前，我们必须先了解git的分区：

- 工作区（working directory） 
- 暂缓区（stage index） 
- 历史记录区（history）
- 三个区域关系：工作区是我们能看到的区域，我们在工作区修改增加代码；完成编辑后，我们用git add 将工作区文件添加到暂存区；然后利用git commit 提交文件到我们自己的分支。

## 增删改查:

		git add <文件名>                 -- 工作区文件添加到暂存区
		git add .                       -- 工作区所有修改动态添加到暂存区
		git commit	-m "提交说明"        -- 提交git add后的文件到我们自己的分支
		git status						-- 掌握仓库当前的状态(*红色*代表未add未commit,*绿色*代表未commit)
		git diff <文件名>                -- 能查看该文件具体修改了什么内容
		git checkout --<文件名>          -- 使该文件在工作区的修改全部撤销(这有两种情况:1.该文件自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态 2.该文件已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态)
		git reset HEAD <文件名>          -- 可以把暂存区的修改撤销掉（unstage）
		git rm <文件名>  

## 版本:
		git log							-- 显示从最近到最远的提交日志
		git log --pretty=oneline        -- 显示从最近到最远*简化*的提交日志
		git reset --hard HEAD^          -- 显示当前版本
		cat <文件名>                     -- 显示当前文件的内容
		git reset --hard <commit id>    -- 退回commit id 时的版本
		git reflog                      -- 记录你的每一次reset和commit的命令
		git diff HEAD -- <文件名>        -- 查看该文件工作区和版本库里面最新版本的区别


                -- 从版本库中删除该文件
		
## 远程仓库:

		git remote add origin <远程仓库地址> -- 添加一个仓库地址的别名,即origin
		git remote -v 					-- 可以查看对应的*别名*和*远程仓库地址*
		git push -u origin master       -- 把本地仓库push到远程仓库上
		git pull origin master          -- 把远程仓库pull到本地仓库
		git clone <远程仓库地址>          -- 把对应的远程仓库地址的内容clone到本地
		
## 分支:

		git checkout dev                 -- 创建dev分支
		git checkout -b dev 			 -- 创建dev分支并切换到dev分支上
		git branch 						-- 查看所有分支(绿色代表当前分支)
		git checkout <分支名>            -- 切换到对应的分支上
		git merge <分支名>               -- 把对应分支合并到当前分支上
		git brand -d <分支名>            -- 删除对应的分支
		


## 分支图:

	
		git log --graph                -- 查看分支合并图


## 分支管理：

说明：

- Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。

- 如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。


		git merge --no-ff -m "说明修改" <分支名>    -- --no-ff参数，表示禁用Fast forward -m "说明修改" 代表一个commit


## bug分支：

情境：
当你接到在master主分支上有修复bug的任务代号:bug_001,想创建一个分支bug_001来修复它，但当前正在dev上进行的工作还没有提交：


		git stash                           		   -- 把当前工作现场“储藏”起来，等以后bug_001修复好后继续工作
		git checkout master          			       -- 回到master主分支 
		git ckeckout -b bug_001      			       -- 创建bug_001分支并切换到bug_001分支上
		git add .                     	               -- 添加修复 
		git commit -m "修复bug_001"  			       -- 提交bug_001修复
		git checkout master                            -- 回到master主分支 
		git merge --no-ff -m "merged bug_001" bug_001  -- 合并bug_001分支到master主分支上并commit"merged bug_001"日志
		git checkout dev							   -- 切换到dev分支上
		git stash list 								   -- 查看“储藏”起来工作现场

		【*git stash apply							       -- 修复储藏”起来工作现场（但是恢复后，stash内容并不删除，你需要用git stash drop来删除）
		git stash pop  								   -- 修复储藏”起来工作现场（恢复的同时把stash内容也删了）*】

		git stash apply stash@{0}                      -- 恢复指定的stash


## feature分支：

情境：
软件开发过程中，添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。现在，有一个新的功能需要实现，代号为newfunction。


		git checkout -b feature_newfc				-- 创建feature_newfc分支并切换到feature_newfc准备开发
		git add newfunction.py						-- 开发完成并添加到暂存区
		git commit -m "add newfunction"				-- 提交到feature_newfc上
		git chekout master							-- 切换到master主分支上，准备合并分支
		git merge feature_newfc                     -- 合并feature_newfc分支到master主分支

新情境：

刚刚开发的新功能，因经费不足，新功能必须取消！（没有开发完成！还没有合并到master主分支）如何就地销毁呢？
		
		git bracnch -d feature_newfc 				-- 准备销毁feature_newfc分支，但因为还没有合并报错，就地销毁失败！
		git bracnch -d feature_newfc				-- 强行销毁，销毁成功！

## 标签管理

说明：

- Git中打标签非常简单，首先，切换到需要打标签的分支上，
- 创建的标签都只存储在本地，不会自动推送到远程，所以，打错的标签可以在本地安全删除。
- 如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除，然后，从远程删除。删除命令也是push


		git branch                                     		-- 查看所有分支（带绿色*的是当前分支）
		git checkout master									-- 切换到master主分支上
		git tag <新建标签名>									-- 新建一个标签
		git tag 											-- 查看所有标签
		git log --pretty=oneline --abbrev-commit			-- 查询历史提交的commit id
		git tag <tag commit id>   							-- 回到指定的tag
		git show <tagname>   								-- 查看标签信息
		git tag -a v0.1 -m "version 0.1 released"      	    -- 创建带有说明的标签，用-a指定标签名，-m指定说明文字   	
		git tag -s v0.2 -m "signed version 0.2 released" 	-- 通过-s用私钥签名一个标签（签名采用PGP签名，因此，必须首先安装gpg（GnuPG），如果没有找到gpg，或者没有gpg密钥对，就会报错！如果报错，请参考GnuPG帮助文档配置Key！）
		git tag -d <tagname>								-- 删除标签
		git push origin <tagname>							-- 推送某个标签到远程
		git push origin --tags								-- 一次性推送全部尚未推送到远程的本地标签

		【*git tag -d <tagname>								-- 标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除
		git push origin :refs/tags/<tagname>				--然后，从远程删除。也是push*】


## 自定义Git

		git config --global color.ui true					-- 让Git显示颜色，会让命令输出看起来更醒目(一般默认不用自己再配置！)
		其它配置指令请百度，这里不一一指出！

## 别名
		git config --global alias.co checkout				-- 等同 git co dev
		git config --global alias.ci commit				-- 等同 git ci -m "提交说明"
		git config --global alias.br branch				-- 等同 git br 
		...


![](https://github.com/aqqje/images/raw/master/images/github.jpg "github")
### 注意：所有的尖括号皆可以省略！


-----


- [参考1](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
- [参考2](http://www.jianshu.com/p/ddac9d030d2c)
- [参考3](https://www.cnblogs.com/liujiaq/p/5670069.html)

作者：梦不若星辰
链接：https://aqqje.github.io
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
