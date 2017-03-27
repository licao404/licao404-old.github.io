---
title: jQuery懒加载
date: 2016-04-25 19:49:41
categories:
  - 编程学习
  - 前端基础
tags:
  - JavaScript
  - jQuery
  - 效果
---
<blockquote class = "blockquote-center">对于图片过多的页面，我们会使用懒加载，具体表现为当页面被请求时，只加载可视区域的图片，其它部分的图片则不加载，只有这些图片出现在可视区域时才会动态加载这些图片，从而节约了网络带宽和提高了初次加载的速度，也提高了用户体验。本文将介绍其实现原理以及如何动手实现。</blockquote>

<!--more-->

>### 【原理准备】

#### **1. 如何判断一个元素是否出现在窗口可视范围（浏览器的上边缘和下边缘之间，肉眼可视）。写一个函数      `isVisible` 实现**
![][1]
依照我们画的思路图，给出描述程序：
```javascript
//判断元素是否出现在浏览器窗口可视范围
function isVisiable ($node) {
	 var scrollT = $(window).scrollTop(),//获取页面顶部到窗口顶部的滚动距离
	 	 windowH = $(window).height(),//获取浏览器窗口高度
	 	 offsetT = $node.offset().top;//获取该元素距页面顶部的距离

	 if (scrollT+windowH > offsetT) {
	 	return true;
	 }else{
	 	return false;
	 }
}
```

#### **2. 当窗口滚动时，判断一个元素是不是出现在窗口可视范围。每次出现都在控制台打印 true 。用代码实现**
>[demo][2]

```javascript
var $target = $('.nav3');

//监视窗口滚动
$(window).on('scroll',function () {
    var result = isVisiable ($target);
    if(result){console.log(result)};
})  

//判断元素是否出现在浏览器窗口可视范围
function isVisiable ($node) {
  var scrollT = $(window).scrollTop(),//获取页面顶部到窗口顶部的滚动距离
      windowH = $(window).height(),//获取浏览器窗口高度
      offsetT = $node.offset().top;//获取该元素距页面顶部的距离

  if (scrollT+windowH > offsetT) {
    return true;
  }else{
    return false;
  }
}
```

#### **3. 当窗口滚动时，判断一个元素是不是出现在窗口可视范围。在元素第一次出现时在控制台打印 true，以后再次出现不做任何处理。用代码实现**
>[demo][3]

```javascript
console.log("当nav3出现时打印true");
var $target = $('.nav3');

function isVisiable ($node) {
  var scrollT = $(window).scrollTop(),//获取页面顶部到窗口顶部的滚动距离
      windowH = $(window).height(),//获取浏览器窗口高度
      offsetT = $node.offset().top;//获取该元素距页面顶部的距离

  //判断该元素是否出现过，出现过就不再进行
  if ($target.data('isAppeared')) {
  	return;
  }

  if (scrollT+windowH > offsetT) {
    return true;
  }else{
    return false;
  }
}

var clock;
$(window).on('scroll',function () {
	if (clock) {
		clearTimeout(clock);
	}
	clock = setTimeout(function () {
	    var result = isVisiable ($target);

	    if(result){
	    	console.log(result);
	    	$target.data('isAppeared',true);
	    };    		  
	},300);

});
```

#### **4.图片懒加载的原理是什么？**
原理实际上很简单，当页面被请求时，只加载可视区域的图片，其它部分的图片则不加载，只有这些图片出现在可视区域时才会动态加载这些图片。判断图片是否出现在可视区域内前文已通过函数实现；还有一个就是当页面加载时我们需要将页面上的 `img` 标签的 `src` 指向一个小的图片（随便是啥），把我们要展示图片的真实地址放在一个自定义属性中，如 `data-src`：

```html
<img src="loading.gif" data-src="http://xx.oo.com"/>
```

>### 【具体实现】

#### **1. 实现如下回到顶部效果**
当页面滚动到一定距离时，窗口右下角会出现回到顶部按钮，点击按钮页面会滚动到顶部。

- [效果预览][4]
- [实现代码][5]

#### **2. 实现图片懒加载效果**

- [效果预览][6]
- [实现代码][7]


#### **3. 实现如下无限滚动效果**

- [效果预览][8]
- [实现代码][9]


---

[1]:http://7xr868.com1.z0.glb.clouddn.com/task29/isVisible.png
[2]:http://js.jirengu.com/fufo/8/edit?html,console,output
[3]:http://js.jirengu.com/lif/1/edit?html,console,output
[4]:http://febox.applinzi.com/task29/task29-1.html
[5]:https://github.com/licao404/landemo/blob/master/task29/js/backtop.js
[6]:http://febox.applinzi.com/task29/task29-2.html
[7]:https://github.com/licao404/landemo/blob/master/task29/task29-2.html
[8]:http://febox.applinzi.com/task29/task29-3.html#
[9]:https://github.com/licao404/landemo/blob/master/task29/task29-3.html
