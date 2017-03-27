---
title: BFC
categories:
  - 编程学习
  - 前端基础
tags:
  - CSS
date: 2016-02-28 13:32:34
---
>### 【基础知识】

#### **1. 在什么场景下会出现外边距合并？如何合并？如何不让相邻元素外边距合并？**
> 1. 相邻块盒子的垂直外边距合并只有他们是在同一BFC，且没有padding与border将外边距隔开才会发生；
2. 垂直相邻元素，子元素和父元素会发生合并，合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者。
3. 创建另外一个BFC，让不想发生外边距合并的元素处于不同的BFC。

<!--more-->
#### **2. 去除`inline-block`内缝隙有哪几种常见方法?**
* 1.修改`li`的代码书写格式：
    * ![li书写1][1]

    * ![li书写2][2]


* 2.用`负margin`：
  * ![-margin][3]


* 3.用浮动并在父元素用`overflow:auto`清除浮动：
  * ![float][4]


* 设置父元素的`font-size`为`0`,子元素再给一个`font-size`:
  * ![父font-size][5]


* 适配IE8以下加hack语句[`*display:inline`][6]

#### **3. 父容器使用`overflow: auto| hidden`撑开高度的原理是什么？**
> 给父容器添加`overflow: auto| hidden`样式即触发了BFC，这时如果父容器内只有浮动元素，则会撑开父容器高度。


#### **4. BFC是什么？如何形成BFC，有什么作用?**
> 1. 定义：浮动元素和绝对定位元素，非块级盒子的块级容器（例如 inline-block, table-cells, 和 table-captions），以及overflow值不为“visiable”的块级盒子，都会为他们的内容创建新的BFC（块级格式上下文）。
2. 如何形成：可以通过给父容器添加一个触发BFC的样式即可。例如`float:left`,`position:absolute`,`overflow:hidde`,`display:inline-block`等；（用来清除浮动可能带来一些副作用，一般使用`overflow:auto`）
3. * 用于`包含浮动`。如果父容器内存在浮动元素，则父容器没有高度，它的浮动孩子将会脱离文档流，影响到其他元素的排列。这时可以创建新的BFC，撑开父容器的高度可以包含子元素；
* 用于`防止边距合并`，实质上外边距合并是由BFC导致的（[详细][7]），毗邻块盒子的垂直外边距折叠只有他们是在同一BFC时才会发生，如果他们属于不同的BFC，他们之间的外边距将不会折叠。所以通过创建一个新的BFC我们可以防止外边距折叠；
* 用于`防止文字环绕`；
* 用于`多列布局`；当我们设计一个多列布局占满宽度时，最后一列被挤到下一行，在其中一个列的布局中建立了一个新的BFC，它将会在前一列填充完之后的后面占据所剩余的空间。

#### **5. 浮动导致的父容器高度塌陷指什么？为什么会产生？有几种解决方法**
> 概念：没有设置父容器的高度或者设置了`auto`，容器内只含有浮动元素，则他们的父容器会没有高度属性，之后的元素会“挤上来”，造成“高度塌陷”。

> * 解决：
1. 给父容器添加`overflow: auto| hidden`等样式触发BFC，撑开父容器高度。
2. 使用伪类`:after`,父容器通过`:after`来为其内容末尾添加一个内容为空的块框，利用这个块框来清除浮动（需要hack兼容IE），`常常用这种方法`!
3. 给父容器后添加一个新标签，用这个新标签来清除浮动撑开高度，兼容性好，但是无语义；



#### **6. 以下代码每一行的作用是什么？ 为什么会产生作用？ 和BFC撑开空间有什么区别？**
```css
  .clearfix:after{      /*添加`:after`伪类*/
    content: '';        /*内容是空的*/
    display: block;     /*空的内容生成块框*/
    clear: both;    /*清除两边浮动*/
}
.clearfix{
    *zoom: 1;   /*IE的hack语句，为了兼容IE6,7，触发IE的hasLayout特有属性，但不会影响页面的显示效果*/
}
```
> 作用：在容器内部最后一个元素创建了一个空的块框，而该块框两则清除了浮动，容器外的其他元素就无法向上挤占了；

区别：BFC是包含容器内的浮动元素，形成一个封闭的小宇宙，不受外界干扰挤占；而这种方法是通过添加空块来清除浮动，不让其他元素上来，二者的实现思路是完全不一样的；


---
>### 【代码】
> #### [task-11-1](https://github.com/licao404/landemo/blob/master/task11/task-11-1.html)
> #### [task-11-2](https://github.com/licao404/landemo/blob/master/task11/task-11-2.html)






  [1]: http://7xr868.com1.z0.glb.clouddn.com/task11%E5%8E%BB%E7%BC%9D%E9%9A%99li.gif
  [2]: http://7xr868.com1.z0.glb.clouddn.com/task11%E5%8E%BB%E7%BC%9D%E9%9A%99li2.gif
  [3]: http://7xr868.com1.z0.glb.clouddn.com/task11%E5%8E%BB%E7%BC%9D%E9%9A%99%E8%B4%9Fmargin.gif
  [4]: http://7xr868.com1.z0.glb.clouddn.com/task11%E5%8E%BB%E7%BC%9D%E9%9A%99%E6%B8%85%E9%99%A4%E6%B5%AE%E5%8A%A8.gif
  [5]: http://7xr868.com1.z0.glb.clouddn.com/task11%E5%8E%BB%E7%BC%9D%E9%9A%99fontsize.gif
  [6]: http://js.jirengu.com/quvohojade/2/edit
  [7]: http://www.w3cplus.com/css/understanding-block-formatting-contexts-in-css.html

---
