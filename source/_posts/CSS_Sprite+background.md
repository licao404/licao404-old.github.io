---
title: CSS Sprite/background
categories:
  - 编程学习
  - 前端基础
tags:
  - CSS
date: 2016-02-26 13:27:33
---
>### 【知识点】

#### **1. CSS Sprite(雪碧图)指什么? 有什么作用？**
* `CSS Sprite`又名`CSS精灵`，一种将零星的小图片合并成一张大图的网络图像应用处理方式，再利用`CSS`的`background-image`,`background-repeat`,`background-position`进行精确定位所需的小图位置;

<!--more-->
* 主要作用：
 * 可以大量减少页面的http请求，提升页面性能，因为如果是每一张图片都去请求一次，浪费的网络资源是不可想象的；
 * 减少图片的字节；
 * 其他作用和弊端见[度娘百科](http://baike.baidu.com/link?url=WNMBLXrej-HoKJ4OspqtsJdTfgc5_jTRO6a3rcthEVyyKfv2M2jGG4lKlOw69NK1KTjHAPmJb4NttvAhzroB6q)。

#### **2. img标签和CSS背景使用图片在使用场景上有何区别?**
> * 如果内容（对每个访问者）是恒定不变的，如icon，logo，等，则选择css背景图：
![icon][1]
![logo][2]

---
> * 如果是变化的内容，比如电商网页上的商品广告（基本上直观理解的图片）都是img标签，还有头像等，他们都是外链到数据库的，可能发生改变：
![img1][3]
![img2][4]


#### **3. title 和 alt属性分别有什么作用?**
* `alt属性`是在你的图片因为某种原因不能加载时在页面显示的提示信息(临时代替图片的作用)，它会直接输出在原本加载图片的地方；
* `title属性`是在你鼠标悬停在该图片上时显示一个小提示，鼠标离开就没有了，HTML的绝大多数标签都支持`title属性`，`title属性`就是专门做提示信息的，比如：![title][5]

#### **4. `background: url(abc.png) 0 0 no-repeat;`这句话是什么意思**
> 设置背景图片为`abc.png`,要显示位置是水平0，垂直0，重复方式是不重复

#### **5. background-size有什么作用? 兼容性如何? 常用的值是?**
* `background-size`可以设置css背景图片的大小，以长度值和百分比显示；也可以根据背景原点位置`background-origin`设置图片的覆盖范围。
* 在[Caniuse](http://caniuse.com/#search=background-size)上查到兼容性为：
  * ![background-size兼容性][6]
  * 让`IE6,7,8`兼容`background-size`的方法：使用IE的滤镜
 ``` css
<style>
.background {
    background-image:url('http://7xr868.com1.z0.glb.clouddn.com/task6inline-block%E6%94%AF%E6%8C%81.png');
    background-size: cover;
    filter: progid:DXImageTransform.Microsoft.AlphaImageLoader(
    src='http://7xr868.com1.z0.glb.clouddn.com/task6inline-block%E6%94%AF%E6%8C%81.png',
    sizingMethod='scale');
}
</style>
 ```

*  常用的值:

  * `auto`：默认值，不改变背景图片的原始高度和宽度；
  * `length`：第一个值为设置图片宽度，第二个值为图片的高度；但是不管是用什么值，都不能为负值。假如只给定一个值，那么第二个自动的为 `auto`；
  * `percentage`：以父元素的百分比来设置背景图像的宽度和高度。设置一个值，同上；
  * `cover`： 将图片等比例缩放来填满整个容器，加入容器不足以包含背景图片，则一些部分不会显示在容器中；
  * `contain`：按比例调整背景图片，使得其图片宽高比自适应整个元素容器的宽高比，因此假如指定的图片尺寸过大，而容器的整体宽高不能恰好包含背景图片的话，那么其容器某些区域可能会有空白。


#### **6. 如何让一个div水平居中？如何让图片水平居中?**
1. 设置div的左右`margin-left`,`margin-right`值为`auto` >>[代码演示](http://js.jirengu.com/laziqaqaki/3/edit)；
2.
  * 给图片加一个父容器，设置容器样式`text-align:center`>>[代码演示](http://js.jirengu.com/yudajuveye/2/edit)；
  * 将img用`display:block`变为块级元素，然后设置`左右margin`值为`auto`，类似于1中设置`div`水平居中>>[代码演示](http://js.jirengu.com/vuyuliseni/1/edit)；


#### **7. 如何设置元素透明? 兼容性？**
> 1. `opacity：number`属性(CSS3属性)指定了一个元素的透明度,默认`number`是1（不透明），0是完全透明，[（更多）](https://developer.mozilla.org/zh-CN/docs/Web/CSS/opacity)。兼容性：![opacity兼容性][7]
2. `filter:alpha(opacity=number);`是专门给IE设定的属性，`number`取值的范围在`0-100`之间；兼容性：（一般用在IE8及以下）![filter兼容性][8]
3. 一段常用代码设置元素透明度：

``` html
.transparent_class {
      filter:alpha(opacity=50);
      -moz-opacity:0.5;/*-moz-opacity是为了兼容一些老版本的Mozilla浏览器*/
      -khtml-opacity: 0.5;/*-khtml-opacity是为了兼容一些老版本的Safari浏览器*/
      opacity: 0.5;
}
```
> 4.[更详细的实现方案](http://www.jb51.net/css/24765.html)

#### **8. opacity 和 rgba都能设置透明，有什么区别?**
> `opacity`作用于`元素`，以及元素内的所有`内容`的透明度，而`rgba()`只作用于元素的`颜色`或其`背景色`。

---
>### 【代码】
> #### [task-9-1](https://github.com/licao404/landemo/blob/master/task9/task-9-1.html)
> #### [task-9-2](https://github.com/licao404/landemo/blob/master/task9/task-9-2.html)
> #### [task-9-3](https://github.com/licao404/landemo/blob/master/task9/task-9-1.html)


  [1]: http://7xr868.com1.z0.glb.clouddn.com/task9%E8%83%8C%E6%99%AF%E5%9B%BE2.png
  [2]: http://7xr868.com1.z0.glb.clouddn.com/task9%E8%83%8C%E6%99%AF%E5%9B%BE1.png
  [3]: http://7xr868.com1.z0.glb.clouddn.com/task9img.png
  [4]: http://7xr868.com1.z0.glb.clouddn.com/task9img2.png
  [5]: http://7xr868.com1.z0.glb.clouddn.com/task9title.png
  [6]: http://7xr868.com1.z0.glb.clouddn.com/task9background-size%E5%85%BC%E5%AE%B9%E6%80%A7.png
  [7]: http://7xr868.com1.z0.glb.clouddn.com/task9opacity%E5%85%BC%E5%AE%B9%E6%80%A7.png
  [8]: http://7xr868.com1.z0.glb.clouddn.com/task9filter%E5%85%BC%E5%AE%B9%E6%80%A7.png

---
