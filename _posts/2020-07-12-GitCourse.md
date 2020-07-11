---
layout: post
title: "Git教程"
date: 2020-07-12   
tag: Git 
---

### 介绍       

　　同生活中的许多伟大事物一样，Git 诞生于一个极富纷争大举创新的年代。

　　Git的作者Linus，同样是Linux 的缔造者，为了高度推举开源的大旗和更好的开发和管理Linux，开发出了Git这个优秀的版本版本控制系统

　　作为一位开发者，熟悉掌握Git是一项必备技能，如果连自己编写的代码都不能做到很好的控制，那我们还能写出什么优秀的代码。现在基于Git的版本控制软件有很多，比如：Sourcetree、TortoiseGit等工具，但是本人还是推荐使用命令行操作，因为只有熟练的使用命令行，才能更好的理解底层原理。          

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


### 常见问题： 
- 配置了.gitignore的忽略项但是不起作用，解决办法：清理本地库的缓存，并且重新添加版本库。
````
git rm -r --cached .
git add .
git commit -m "update .gitignore"
````

<br>

转载请注明：[刘强的博客](https://liuqiang-code.github.io/) » [点击阅读原文](https://liuqiang-code.github.io/2020/07/GitCourse/)     

