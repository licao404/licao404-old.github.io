---
title: CSS选择器
categories:
  - 编程学习
  - 前端基础
tags:
  - CSS
date: 2016-02-25 13:14:52
---
>### 【基础知识】

#### **1. CSS选择器常见的有几种?**
> 通配选择器(`*`)、标签选择器(`element`)、id选择器(`#id`)、类选择器(`.class`)、伪类选择器(`E:hover`等)、属性选择器(`E[atter=value]`等)、组合选择器(`E>F`等)、伪元素选择器(`E::first-letter`等)

<!--more-->
#### **2. 选择器的优先级是怎样的?**
> 一般而言，选择器越特殊，它的优先级越高。也就是选择器指向的越准确，它的优先级就越高。

  * 常见选择器优先级排序（从高到低）：
   1. 在属性后面使用 `!important` 会覆盖页面内任何位置定义的元素样式；
   2. 作为style属性写在元素标签上的内联样式；
   3. id选择器；
   4. 类选择器；
   5. 伪类选择器；
   6. 属性选择器；
   7. 标签选择器；
   8. 通配选择器；
   9. 浏览器自定义

* CSS规则由多个选择器组成,通常我们用1表示标签名选择器的优先级，用10表示类选择 器的优先级，用100标示ID选择器的优先级。比如`.polaris span {color:red;}`的选择器优先级是 10 + 1 也就是11；而 `.polaris`的优先级是10；浏览器自然会显示红色的字。
* 如果两个选择器规权值就是一样,后面的覆盖前面的！如下`div`文案的颜色为`#666`  
``` css
div {color: #333;}
....
div {color: #666;}
```

#### **3. class 和 id 的使用场景?**
 > * 在对页面排版进行结构化布局时（比如说通常一个页面都是由一个页眉，一个报头<masthead>，一个内容区域`content`和一个页脚`footer`等组成），一般使用id比较理想，因为一个id在一个文档中只能被使用一次。而这些元素在同一页面中 很少会出现大于一次的情况。
* class更多的被应用到文字版块以及页面修饰等方面


#### **4. 使用CSS选择器时为什么要划定适当的命名空间？**
* 使用适当的命名空间可以让选择器只对所选的命名空间有效，避免与其他元素冲突。

#### **5. 以下选择器分别是什么意思?**

```css
#header{        /*定位id为header的元素*/
}
.header{        /*定位class为header的元素*/
}
.header .logo{
}               /*定位class为header元素的后代中所有class为logo的元素*/
.header.mobile{ /*同时定位class为header的元素和class为mobile的元素*/
}
.header p, .header h3{/*同时定位class为header元素下所有的p元素和h3元素*/
}
#header .nav>li{ /*id为header的元素后代中class为nav元素的直接li元素*/
}
#header a:hover{ /*id为header元素的后代中所有a标签的hover状态*/
}
```

#### **6. 列出你知道的伪类选择器**
- `E:hover`鼠标悬浮在上的元素
- `a:link`鼠标未被点击的链接
- `E:active`鼠标按住的元素
- `a:visited`鼠标已点击的链接
- `E:focus`获得焦点的
- `E:first-child`E的父元素的第一个子元素
- `E:nth-child(n)`E的父元素下第n个子元素
- `E:first-of-type`E的父元素下使用同种标签的第一个元素
- `E:nth-of-type`E的父元素下使用同种标签的第n个元素  


#### **7. `:first-child`和`:first-of-type`的作用和区别?**
> `:first-child`定位的是其父的第一个子元素；
`:first-of-type`定位的是其父下同种标签的第一个子元素；
```
<body>
    <h3>我是h3</h3>
	<p>我是p1</p>
	<div class="div1">
		<p>我是p3</p>
		<p>我是p4</p>
		<p>我是p5</p>
	</div>
</body>
```
```
p:first-child {border: 1px solid red;}
/*此时定位的只是p3而没有p1*/
p:first-of-type {border: 1px solid red;}
/*此时定位的是p1，p3*/
```

#### **8. 运行如下代码，解析下输出样式的原因。**

``` html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>first-child  vs first-of-child</title>
  <style>
    /*选中.item1,该元素是它父亲的第一个孩子*/
    .item1:first-of-type{
      background: red;
    }

    /*选中.item1,该元素是它父亲所有的 .item1孩子中的第一个*/
    .item1:first-child{
      color: blue;
    }
  </style>
</head>
<body>

 <div class="item1">item1</div>
 <div class="ct">
   <div class="item2">ct-item2</div>
   <div class="item1">ct-item1</div>
   <div class="item1">ct-itmm1</div>
   <div class="item2">
     <div class="item1">ct-item2-item1</div>
   </div>
 </div>
</body>
</html>
```
> * `.item1:first-child{color: blue;}`定位的是class为item1元素的父元素的第一个子元素，所以`item1`、
`ct-item2-item1`有效,文本为蓝色;
*  `.item1:first-of-type{background: red;` 定位clss为`item1`且为父亲的第一个子元素，则只有`item1`、
`ct-item2-item1`有效，背景为红色

#### **9. `text-align: center`的作用是什么，作用在什么元素上？能让什么元素水平居中**
> * 让元素水平居中
* 作用在块级元素上
* 让块级元素内的行内元素水平居中

#### **10. 如果遇到一个属性想知道兼容性，在哪查看?**
> 可以在网站[Caniuse](http://caniuse.com)查询浏览器兼容性！




---

>### 【代码】
> #### [task-8-1](https://github.com/licao404/landemo/blob/master/task8/task-8-1.html)
> #### [task-8-2](https://github.com/licao404/landemo/blob/master/task8/task-8-2.html)

---
