---
layout: post
title: "Nginx之proxy_pass指令"
date: 2020-10-26   
tag: Nginx 
---

### 结论
　 在nginx中配置proxy_pass代理转发时如果在proxy_pass后面的url加/，表示绝对根路径；如果没有/，表示相对路径，把匹配的路径部分也给代理走。

### 验证过程
#### 步骤一：启动被代理服务
- 使用python3提供的 http.server 模块创建一个http服务，并且在该服务下面存放一些资源，以供被请求调用。
<img src="/images/posts/NginxProxyPass/httpServer.png">

- 通过界面访问验证服务是否正常启动
<img src="/images/posts/NginxProxyPass/checkHttpServer.png">

#### 步骤二：配置nginx
- 配置location和upstream模块
<img src="/images/posts/NginxProxyPass/nginxConfig.png">

- 验证nginx是否正常启动
<img src="/images/posts/NginxProxyPass/checkNginx.png">

#### 步骤三：测试验证
- 验证相对路径访问

````
# 访问相对路径 test/a.txt  实际转发路径: http://127.0.0.1:20001/test/a.txt
>>> import requests
>>> URL = r'http://127.0.0.1:20001/'
>>> ret = requests.get(URL + 'test/a.txt')
>>> ret.text
'a file\n'
>>>
````

- 验证绝对路径访问

````
# 访问绝对路径 test2/test/a.txt 实际转发路径 http://127.0.0.1:20001/test/a.txt
>>> import requests
>>> URL = r'http://127.0.0.1:20001/'
>>> ret = requests.get(URL + 'test2/test/a.txt')
>>> ret.text
'a file\n'
>>>

>>> import requests
>>> URL = r'http://127.0.0.1:20001/'
>>> ret = requests.get(URL + 'test3')
>>> ret.text
'd file\n'
>>>
````

<br>
转载请注明：[刘强的博客](https://liuqiang-code.github.io/) » [点击阅读原文](https://liuqiang-code.github.io/2020/10/NginxProxyPass/)