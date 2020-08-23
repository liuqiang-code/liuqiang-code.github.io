---
layout: post
title: "Git笔录"
date: 2020-07-12   
tag: Git 
---

### 介绍       

　　同生活中的许多伟大事物一样，Git 诞生于一个极富纷争大举创新的年代。

　　Git的作者Linus，同样是Linux 的缔造者，为了高度推举开源的大旗和更好的开发和管理Linux，开发出了Git这个优秀的版本版本控制系统

　　作为一位开发者，熟悉掌握Git是一项非常实用的技能，合理高效的管理代码，不仅可以为我们节约时间，还能提高编码的效率。现在基于Git的版本控制软件有很多，比如：Sourcetree、TortoiseGit等工具，但是本人还是推荐使用命令行操作，因为只有熟练的使用命令行，才能更好的理解底层原理。          

### 安装Git
参考[官方安装教程](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git)

### 配置Git      

安装完成后，还需要最后一步设置，在命令行输入：
````
git config --global user.name "User Name"
git config --global user.email "email@example.com"
````      
--global：使用该参数时，Git会修改 ~/.gitconfig 或 ~/.config/git/config 文件里面的用户名和邮箱地址。该配置会默认作用于所有Git管理的仓库，如果需要更改某个仓库下的用户名和邮箱信息，可以使用 git config 命令，不携带 --global 选项，也可以手动修改 .git/config 文件里面用户和邮箱信息。



### 常用命令：
 
>* git init（创建Git版本库）      
>* git add .（全部添加到暂存区）    
>* git commit -m 'commit message'（提交暂存区的记录到本地仓库）
>* git config (配置和获取Git环境参数)  
>* git status (查看版本库状态)
>* git fetch (拉取远程仓库引用到本地版本库)
>* git pull (拉取远程仓库引用到本地工作区)  
>* git push (推送本地引用)
>* git checkout (检出操作)
>* git reflog (查看分支的操作记录) 
>* git stash (暂存本地修改) 


### 常见问题： 

#### 配置了.gitignore的忽略项但是不起作用
````
# 清理本地库的缓存，并且重新添加版本库。
git rm -r --cached .
git add .
git commit -m "update .gitignore"
````

#### 冲突处理流程：
	1. 本地进行了一次提交操作，这个时候我们准备推送到远程仓库，当执行推送操作时GIT提示报错，要我们先执行拉取操作。
	````
	➜  GitTest git:(master) git push
	To https://github.com/liuqiang-code/GitTest.git
	 ! [rejected]        master -> master (non-fast-forward)
	error: failed to push some refs to 'https://github.com/liuqiang-code/GitTest.git'
	hint: Updates were rejected because the tip of your current branch is behind
	hint: its remote counterpart. Integrate the remote changes (e.g.
	hint: 'git pull ...') before pushing again.
	hint: See the 'Note about fast-forwards' in 'git push --help' for details.
	➜  GitTest git:(master)
	````
	2. 根据Git提示执行git pull操作
	````
	➜  GitTest git:(master) git pull
	Auto-merging hello.txt
	CONFLICT (content): Merge conflict in hello.txt
	Automatic merge failed; fix conflicts and then commit the result.
	➜  GitTest git:(master) ✗ 
	````
	3. GIT提示：尝试自动合并hello.txt文件失败，提示要我们手动解决冲突然后再次提交。
	4. 根据提示，首先查看冲突文件 hello.txt，GIT使用 <<<<<<< ======= >>>>>>> 标记冲突行。<<<<<<< HEAD 和 ======= 之间的是本地记录，而 ======= 和 >>>>>>> c53ee6465cc033d1eb38c333612bc98e444afe1f 之间的是远程记录。
	````
	➜  GitTest git:(master) ✗ cat hello.txt
	hello git
	<<<<<<< HEAD
	hello weiwei
	=======
	BigStrong is Handsome
	>>>>>>> c53ee6465cc033d1eb38c333612bc98e444afe1f
	➜  GitTest git:(master) ✗
	````
	5. 然后我们再根据实际要求手动处理冲突，当我们手动处理完冲突后使用git status 查看版本库状态时会提示我们再次执行add和commit操作，执行完commit操作后提示fix conflicts，GIT默认（是否解决只有自己知道）我们解决了冲突，因为我们执行了commit操作，最后查看状态提示执行推送操作。
	````
	➜  GitTest git:(master) ✗ cat hello.txt
	hello git
	hello weiwei and BigStrong is Handsome
	➜  GitTest git:(master) ✗ git status
	On branch master
	Your branch and 'origin/master' have diverged,
	and have 1 and 1 different commits each, respectively.
	  (use "git pull" to merge the remote branch into yours)
	You have unmerged paths.
	  (fix conflicts and run "git commit")
	  (use "git merge --abort" to abort the merge)
	Unmerged paths:
	  (use "git add <file>..." to mark resolution)
		both modified:   hello.txt
	no changes added to commit (use "git add" and/or "git commit -a")
	➜  GitTest git:(master) ✗ git add hello.txt
	➜  GitTest git:(master) ✗ git commit -m "fix conflicts"
	[master 6d705a0] fix conflicts
	➜  GitTest git:(master) git status
	On branch master
	Your branch is ahead of 'origin/master' by 2 commits.
	  (use "git push" to publish your local commits)
	nothing to commit, working tree clean
	➜  GitTest git:(master) git push
	Counting objects: 6, done.
	Delta compression using up to 4 threads.
	Compressing objects: 100% (2/2), done.
	Writing objects: 100% (6/6), 551 bytes | 551.00 KiB/s, done.
	Total 6 (delta 0), reused 0 (delta 0)
	To https://github.com/liuqiang-code/GitTest.git
	   	c53ee64..6d705a0  master -> master
	````
	6. 总结：养成使用 git status 命令习惯。学会看提示信息，GIT 的提示信息非常友善，利用提示信息可以很大的提高工作效率。

#### 远程分支有提交，本地修改未提交可以直接git pull吗？
1、首先通过 git status 查看本地状态
````
➜  GitTest git:(master) git status
On branch master
Your branch is behind 'origin/master' by 2 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)

nothing to commit, working tree clean
```` 
2、修改文件并且查看状态
````
➜  GitTest git:(master) ll
total 8
-rw-r--r--  1 apple  staff    23B  8 23 10:32 hello.txt
➜  GitTest git:(master) echo 'modify file' >> hello.txt
➜  GitTest git:(master) ✗
➜  GitTest git:(master) ✗
➜  GitTest git:(master) ✗ git status
On branch master
Your branch is behind 'origin/master' by 2 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   hello.txt

no changes added to commit (use "git add" and/or "git commit -a")
````
3、不提交修改，直接使用git pull拉取提交
````
➜  GitTest git:(master) ✗ git status
On branch master
Your branch is behind 'origin/master' by 2 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   hello.txt

no changes added to commit (use "git add" and/or "git commit -a")
➜  GitTest git:(master) ✗ git pull
Updating 5be3084..6d705a0
error: Your local changes to the following files would be overwritten by merge:
	hello.txt
Please commit your changes or stash them before you merge.
Aborting
➜  GitTest git:(master) ✗
````
4、总结：git会提示操作用户本地修改未提交直接git pull时本地修改会被覆盖，请先commit或者stash修改。

	
<br>
转载请注明：[刘强的博客](https://liuqiang-code.github.io/) » [点击阅读原文](https://liuqiang-code.github.io/2020/07/GitCourse/)
