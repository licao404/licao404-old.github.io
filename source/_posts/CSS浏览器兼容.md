---
title: CSS浏览器兼容
categories:
  - 编程学习
  - 前端基础
tags:
  - CSS
date: 2016-03-01 13:39:11
---
>### 【知识点】

#### **1. 如何调试 IE 浏览器?**
> 1.通过高版本IE浏览器的开发者工具，可以仿真其之前版本IE浏览器的运行环境，基本上可以临时快速调试各版本IE浏览器：![IE调试01][1]
<!--more-->
2.用虚拟机运行xp等系统，用原生ie6浏览器调试；
3.如果有条件，远程到一台有相应版本浏览器的机器（服务器）上，进行调试；
4.使用第三方调试工具（`IETester`等）模拟各种版本浏览器，不过存在一些未知的问题，毕竟是模拟的；

#### **2. 什么是`CSS hack`？在 CSS 和 HTML里如何写 `hack`？在 CSS 中 ie6、ie7的 `hack` 方式？**
> 由于不同浏览器和某浏览器的不同版本，特别是一些老浏览器，如IE6,7等，对CSS的支持、解析不一致，因此会导致页面的显示效果不一样，而`CSS hack`是利用浏览器漏洞来让不同浏览器显示效果一致，我们把针对不同的浏览器/不同版本写相应的`CSS code`的过程，叫做`CSS hack`；

* `CSS hack`基本上有4种方式：
  * IE条件注释法（IE专有的hack方式，Microsoft官方推荐）

  ```html
  只在IE下生效
  <!--[if IE]>
    <link rel="stylesheet" type="text/css" href="ie.css">
  <![endif]-->

  只在IE6下生效
  <!--[if IE6]>
    <link rel="stylesheet" type="text/css" href="ie6.css">
  <![endif]-->

  只在IE6以上版本生效
  <!--[if gte IE]>
    <link rel="stylesheet" type="text/css" href="ie.css">
  <![endif]-->

  非IE浏览器生效
  <!--[if ！IE]>
    <link rel="stylesheet" type="text/css" href="ie.css">
  <![endif]-->  

  ```
  * 属性前缀法：

    1. `_`和`-`只对`ie6`有效；
    2. `*`对`ie6,7`有效；
    3. `\9`对`ie11`以下所有ie有效；
    4. `\0`对`ie8,9,10`有效；
    5. `\9\0`只对`ie9,10`有效；
    6. `\0\9`只对`ie8,9`有效；
    7. ... ...

  * 选择器前缀法：
    1. `* 前缀`只对IE6有效；
    2. `*+ 前缀`只对IE7有效；
    3. 更多可以参考[browserhacks][2]；

  * 用`JavaScript`判断`ie`版本：

   ```javascript
    // 是否小于等于ie8
        var isIE = '\v'=='v';

    // 判断ie版本
        var ieVersion = (function() { if (new RegExp("MSIE     ([0-9]{1,}[\.0-9]{0,})").exec(navigator.userAgent) != null) { return parseFloat( RegExp.$1 ); } else { return false; } })();
   ```


  #### **3. 列举几种 浏览器兼容问题**
> 1.不同浏览器对标签默认的内外边距不一致，最常见的兼容性问题，解决方式：在`css`开头用通配选择器将所有元素重置`*{margin:0;padding:0;}`;
2.块属性标签float后，又有横行的margin情况下，在IE6显示margin比设置的大,常见的问题体现是ie6中最后一块被顶到下面一行，解决方法：在float的标签样式控制中加入 display:inline;将其转化为行内属性；
3.图片默认有间距，几个img标签放在一起的时候，有些浏览器会有默认的间距，加了问题一中提到的通配符也不起作用；解决方法：用`float`为图片布局；
4.... ...更多兼容性问题参考[chuyuqing的专栏][3]，[文库][4]

#### **4. 针对兼容、多浏览器覆盖有什么看法？渐进增强和优雅降级是什么意思？**
> 1.首先考虑产品的受众，设计师，是否需要兼容老版本的浏览器，比如说该产品用户大多不用IE浏览器，则不需要考虑兼容ie的老版本；
2.看你的`leader`或者审阅产品的人在用什么浏览器,考虑兼容；
3.做到几乎全部的兼容覆盖并不现实，必须结合现有资源，用户实际需求，项目时间的综合考虑是否要做或做什么浏览器的兼容；
4.渐进增强：项目一开始就考虑低版本浏览器的兼容性，保证最基本的功能，再对高级浏览器进行效果改进。
5.优雅降级：一开始就构建完整的功能，完成后再进行低版本浏览器兼容；

#### **5. `reset.css`和`normalize.css`分别是做什么的？为什么推荐使用 nomalize.css?**
> 1.`reset.css`是全局样式重置，html标签在不同浏览器的默认样式存在差异，`reset.css`一开始就将浏览器的默认样式全部暴力覆盖掉；
2.`normalize.css`保留浏览器的原来样式并且做到每个浏览显示一致；
3.`Normalize`相对`平和`，注重通用的方案，重置掉该重置的样式，保留有用的`user agent`样式，同时进行一些 bug 的修复，这点是 reset 所缺乏的,所以通用性和可维护性高；


---
>### 【操作】

#### 1.安装 VirtualBox , 下载 安装虚拟机
![虚拟机][5]
> #### 2.在 ie 6, 7, 8中展示 `盒模型`、`inline-block`、`max-width`的区别

* 测试代码：
``` html
  <style type="text/css">
  *{
  	padding: 0;
  	margin: 0;
  }
    .box{
      display: inline-block;
      height:100px;
      width:100px;
      border:20px solid red;
      padding:30px;
      margin: 40px;
      }
      .box2{
      display: inline-block;
      height:100px;
      width:100px;
      max-width: 50px;
      border:20px solid blue;
      padding:30px;
      margin: 40px;
      }
  </style>

<!--****************************************************-->

<body>
 <div class="box">
  </div>
  <div class="box2"></div>
</body>

```
1.在标准模式下的盒模型如下图所示，`盒子总宽度/高度=width/height+padding+border+margin`;![标准盒子模型][6]
在`ie6,7`怪异模式下,`盒子总宽度/高度=width/height + margin = 内容区宽度/高度 + padding + border + margin`;
2.`inline-block`只有ie8支持，`max-width`ie7,8支持（存在bug具体看[caniuse][7]）：

---
IE8
![ie8][8]
---
IE7
![ie7][9]
---
IE6
![ie6][10]


  [1]: http://7xr868.com1.z0.glb.clouddn.com/task13IE%E5%BC%80%E5%8F%91%E8%80%85.png
  [2]: http://browserhacks.com/
  [3]: http://blog.csdn.net/chuyuqing/article/details/37561313
  [4]: http://wenku.baidu.com/link?url=ysYAKTQTyuudp9fU8sLvVr-VlGdJvCrHFWRpN8RcJSo0hxw-XnBJ_zhiHVudTy7Mq3AQSM6TEmCMDFaS4CNrS2NXGkokWmvU9gKGFaqxVYe
  [5]: http://7xr868.com1.z0.glb.clouddn.com/task13Win_XP.png
  [6]: http://7xr868.com1.z0.glb.clouddn.com/task13ie8%E6%A0%87%E5%87%86%E5%92%8C%E6%A8%A1%E5%9E%8B.png
  [7]: http://caniuse.com/#search=max-width
  [8]: http://7xr868.com1.z0.glb.clouddn.com/task13inline-block-ie8.png
  [9]: http://7xr868.com1.z0.glb.clouddn.com/task13inline-block-ie7.png
  [10]: http://7xr868.com1.z0.glb.clouddn.com/task13ie6.png

---
