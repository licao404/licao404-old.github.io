---
title: 使用XAMPP搭建本地Apache服务器
categories:
  - 计算机基础
  - 计算机网络
tags:
  - php
  - Web服务器
  - Apache
  - 前后端
date: 2016-04-10 16:29:58
photos:
  - http://7xrvo9.com1.z0.glb.clouddn.com/2016.04.10/XAMPP.jpg
---
<blockquote class = "blockquote-center">Web服务器的搭建，例如[Apache][1]，对初学者来说并不是简单事，如果还想添加PHP、MySQL等，就更难了。如果我们借助一些集成软件包，一切操作都简单了。而本文使用的XAMPP，就是这样一个易于安装且包含 MySQL、PHP 和 Perl 的 Apache 发行版，其他类似软件还有[wamp][2]等</blockquote>

### 安装及配置
<!--more-->
**1）**.XAMPP是全平台的，直接在[官网][3]下载系统对应的版本：[XAMPP for Windows][4]  |  [XAMPP for Linux][5]  |  [XAMPP for OS X][6]

**2）**.安装XAMPP，不要安装到C盘，安装过程就不详细展开，不了解的可自行搜索。建议至少安装Apache、MySQL、PHP、以及phpMyAdmin；

**3）**.新建一个文件夹来存放网站代码，比如我在D盘下新建`myWeb`文件夹；

**4）**.安装完成后，打开安装目录下的`apache`文件夹下的`conf`文件夹下的`httpd.conf`文件（记事本或其他文本编辑器）。修改
`DocumentRoot`目录（该目录就是网站代码的路径），大概在240行附近，将其改为刚刚新建的文件夹的路径，例如：
```
DocumentRoot "D:/myWeb"
<Directory "D:/myWeb">
```

**5）**.在`C:\Windows\System32\drivers\etc`目录下修改`host`，加入
```
127.0.0.1 www.licao-train.com
```

>这一步的目的是让你后面在浏览器输入`www.licao-train.com`域名能定位到你的代码目录（这里是`myWeb`目录），当然域名可以替换。

### 测试
**1）**.在存放网站代码文件夹里(这里是myWeb文件夹)新建一个php文件`hello.php`，打开文件编写php代码：
```php
<?php 
    echo "hello php"; 
    phpinfo(); 
?> 
```

**2）**.保存退出，打开XAMPP，启动Apache和MySQL（start）；

**3）**.在浏览器地址栏输入`http://localhost/index.php`或者`http://www.licao-train.com/hello.php`，看是否可以访问刚刚新建的`php`文件：
![][7]

到这里，你的Apache服务器+PHP基本上已经搭建好了。遇到不可测的问题可以在下面回复，我们一起学习。


[1]:http://baike.baidu.com/link?url=MroN_mSBbcJL21A0lYqZlh5FNT3Hukj2ZfurTNf0gxr904iT7hc7zrVaLUC8UuXLTT8ABpm16ilURuOMOMwma9b2R011L2tqMeugghpPt9W
[2]:http://www.wampserver.com/en/
[3]:https://www.apachefriends.org/zh_cn/index.html
[4]:https://www.apachefriends.org/xampp-files/5.6.19/xampp-win32-5.6.19-0-VC11-installer.exe
[5]:https://www.apachefriends.org/xampp-files/5.6.19/xampp-linux-x64-5.6.19-0-installer.run
[6]:https://www.apachefriends.org/xampp-files/5.6.19/xampp-osx-5.6.19-0-installer.dmg
[7]:http://7xr868.com1.z0.glb.clouddn.com/task23/XAMPP%E9%85%8D%E7%BD%AE%E5%B1%95%E7%A4%BA.png

---
