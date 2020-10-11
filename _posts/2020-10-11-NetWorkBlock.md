---
layout: post
title: "web服务之网络堵塞"
date: 2020-10-11   
tag: 计算机网络 
---

### 场景

　 客户反馈现场的服务有时点击没有反应，页面时常抱错。打开浏览器开发者工具发现存在很多请求超时请求。

   <img src="/images/posts/NetWorkBlock/serviceF12.png">

### 处理过程
#### 排查服务自身问题
　 遇到问题首先检查服务运行日志，看是否能根据抱错日志跟踪到原因，查看服务运行日志发现报了一个断开渠道的错误，查询相关资料后得知该错误是tomcat服务处理一个已经关闭的请求时报出的异常。由于服务是通过Nginx代理出来的服务，所以进一步排除Nginx是否存在相关抱错，排查发现不存在相关抱错，所以进一步排除网络是否存在问题。

   <img src="/images/posts/NetWorkBlock/serviceRunningLog.png">

#### 排查网络问题
　 使用telnet命令发现网络不稳定的情况
   <img src="/images/posts/NetWorkBlock/telnet.png">
　 使用ping命令发现网络存在丢包的情况  
   <img src="/images/posts/NetWorkBlock/ping.png">

### 总结
　 web服务出现异常时，有时需要从网络上排查问题，网络是一切服务的必要条件。我们可以通过ping、telnet等命令检查网络的基础状况，如果需要更加深度的排查网络问题的话，我们可以借助Wireshark抓包进行分析。

<br>
转载请注明：[刘强的博客](https://liuqiang-code.github.io/) » [点击阅读原文](https://liuqiang-code.github.io/2020/10/NetWorkBlock/)