---
title: 从URI到页面呈现
categories:
  - 编程学习
  - 后端基础
tags:
  - php
  - Web服务器
---
## 【动手】
#### **1. 安装 `xampp` 套件**
![](http://7xr868.com1.z0.glb.clouddn.com/task23/%E5%AE%89%E8%A3%85XAMPP.png)

#### **2. `xampp`的简单配置使用，在本地启动 web 服务器，通过浏览器访问，通过浏览器打开本地 webserver 下的 php 文件。**
![](http://7xr868.com1.z0.glb.clouddn.com/task23/XAMPP%E9%85%8D%E7%BD%AE%E5%B1%95%E7%A4%BA.png)

#### **3. 新浪云 SAE的使用（支持后端语言），通过 git 上传自己的代码（一个简单的 php 文件）到新浪云,如下是`SAE`应用地址**
- **[http://febox.applinzi.com/](http://febox.applinzi.com/)**

## 【问题】
#### **1. 简单描述下web 服务器、PHP、数据库、浏览器是如何实现动态网站的?**
1. 客户端（浏览器）根据用户输入的`URL`，寻找`DNS`服务器将其解析为对应的Web服务器的`IP`地址，返回给浏览器；

2. 浏览器打包`Http`请求，通过`TCP`协议连接前一步返回的IP所对应的Web服务器，通过默认的`80`端口请求Web服务器上相应目录下的动态语言文件（如index.php）;

3. Web服务器将用户请求的php文件交给php应用服务器处理（Web服务器本身不能处理php动态语言文件）；

4. php应用服务器接收、打开并解释php文件，在php文件中通过对数据库的连接代码连接本机或其他机器上的`MySQL`数据库，在php中执行`SQL`查询语句获得数据，php应用服务器将获得的数据生成`html`静态代码；

5. php应用服务器将生成的`html`静态代码返回`Web`服务器，`Web`服务器通过`TCP`协议将`html`静态代码传给浏览器；

6. 浏览器解析接收到的代码，开始渲染页面并呈献给用户。

#### **2. 常见的 WEB 服务器有哪些？**
Linux/Unix平台
- Apache：
使用最多的Web服务器，几乎可以运行在所有计算机平台，开源免费。简单、速度快、性能稳定，并可做代理服务器来使用。

- Nginx：小型高效，支持正向和反向代理等；


- ……

Windows平台
- IIS：微软主推的Web服务器，IIS提供了一个图形界面的管理工具，称为Internet服务管理器，可用于监视配置和控制Internet服务。

#### **3. 打开浏览器，在地址栏输入 `http://jirengu.com` 页面展现了饥人谷官网的信息，整个过程发生了什么？（饥人谷官网后台语言 php,web服务器 nginx，数据库 mysql）**
1. 通过域名（URL）查询`nginx`服务器对应的IP地址（DNS解析）：
 - 查浏览器缓存，看是否有缓存的DNS,有的话就可以直接使用；
 - 查系统缓存，如果浏览器缓存无记录，浏览器调用系统中的缓存记录；
 - 查路由器缓存，如果系统中无缓存，进一步查询路由器缓存；
 - ISP缓存，如果路由器中无缓存，进一步查询ISP；
 - 递归搜索，如果ISP缓存里仍然查不到，就会从顶级域名服务器的根域名服务器开始递归查询，一定可以查到；

2. 上一步查到的IP地址`121.40.201.213`,浏览器打包`http`请求（服务器所需要的一些信息）,如下图：
![](http://7xr868.com1.z0.glb.clouddn.com/task23/http%E8%AF%B7%E6%B1%82.png)

3. 通过`TCP`协议与`nginx`服务器创建连接（[三次握手](http://www.jellythink.com/archives/705)保证通信可靠性），然后浏览器向服务器发送请求；

4. `nginx`服务器处理浏览器发来的请求，由于后台语言是`php`，则将请求交给php应用服务器；

5. php应用服务器接收、打开并解释php文件，在php文件中通过对数据库的连接代码连接本机或其他机器上的`MySQL`数据库，在php中执行`SQL`查询语句获得数据，php应用服务器将获得的数据生成html静态代码；

6. php应用服务器将生成的html静态代码返回`nginx`服务器，`nginx`服务器通过TCP协议将html静态代码传给浏览器，如下是响应头：
![](http://7xr868.com1.z0.glb.clouddn.com/task23/http%E5%93%8D%E5%BA%94.png)

7. 浏览器接收响应内容，浏览器开始下载并同时渲染内容，顺序都是从上到下，遇到JavaScript就先下载JavaScript，解析完JavaScript再继续进行其他；具体渲染过程如下：
 - 解析`HTML`生成`DOM`树；
 - 解析`CSS`生成`CSSOM`树；
 - 组合`DOM`和`CSSOM`生成渲染树；
 - 遇到`JavaScript`解析JavaScript，阻塞后面的解析和渲染；
 - 最后通过调用操作系统`Native GUI`的`API`进行页面绘制，呈现在用户面前。
