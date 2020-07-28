---
layout: post
title: "SpringBoot默认存储路径带来的BUG"
date: 2020-07-28 
tags: Java  
---

### 背景

　 接到消息说线上一直运行的一个平台出现了Bug，需要排查原因。收到这个消息的时候一脸惊讶，为什么好好运行的平台会突然出问题。排查运行日志发现是无法读取到文件。核实得知线上的工程以前是以war包的形式运行在Tomcat容器中而最近改成了jar包独立运行。

### 原因
  
  经过排查定位错误代码为：
  ````
  ServletContext servletContext = webApplicationConnect.getServletContext();
  ````
  在Linux服务器上SpringBoot通过该方式获取的默认存储路径为 /tmp 

<img src="/images/posts/Tomcat2SpringBootBug/image1.png">

### 分析

- Linux 重启时会自动清理该目录，同时也存在一个定时任务清理，显然文件保存该目录很不合理。
- SpringBoot工程重启后获取的路径是不同的，这就导致每次重启工程后获取文件操作出错。

### 处理方案
　 在SpringBoot工程的application.propertis配置文件中添加一个文件存储路径，所有有关文件的操作都是以该路径为基准。

### 总结
- 根据系统运行日志排除问题
- 当使用一项新技术时需要对该技术有一定的掌控度，切记盲目使用。

<br>
转载请注明：[刘强的博客](https://liuqiang-code.github.io/) » [点击阅读原文](https://liuqiang-code.github.io/2020/07/Tomcat2SpringBootBug/)