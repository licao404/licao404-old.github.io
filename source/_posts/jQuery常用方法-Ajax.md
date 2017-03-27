---
title: 'jQuery常用方法 & Ajax'
date: 2016-04-19 22:57:05
categories:
  - 编程学习
  - 前端基础
tags:
  - JavaScript
  - jQuery
  - AJAX
---
>### 【知识点】

#### **1. Jquery 中， `$(document).ready()` 是什么意思？和  `window.onload` 的区别？ 还有其他什么写法或者替代方法？**
`$(document).ready()` 确保在所有 DOM 构建完成之后，再运行 jQuery 代码，不管其中的代码放在任何位置都是可以运行的，相当于放在了尾部：
```javascript
$(document).ready(function(){
  console.log( "ready!" );
});
```
<!--more-->
>`$(document).ready()` 和 `window.onload` 的区别：

- 执行时间不同。`window.onload` 是等页面所有的资源包括图片等外链资源都加载完毕后才能执行；`$(document).ready()` 只需等到 DOM 构建完成后便可执行，与前者的区别是 DOM 树虽然建立起来，但页面不一定加载完成；

- 可被执行的次数不同。 `window.onload` 不能同时编写多个，如果有多个 `window.onload` 方法，只会执行最后一个 `window.onload`，之前的 `window.onload` 都将被覆盖；`$(document).ready()` 可以同时编写多个，并且都可以得到执行；

- `window.onload` 没有简写形式

>`$(document).ready()` 几种简写形式：

```javascript
$(document(){
  console.log( "ready!" );
})
```
```javascript
$(function() {
    console.log( "ready!" );
});
```
```javascript
$().ready(function(){
  console.log( "ready!" );
});
```


#### **2. `$node.html()` 和 `$node.text()` 的区别?**
- `$node.html()` 获取的是 `$node`内部的HTML内容；
- `$node.text()` 获取的是 `$node`内部的文本内容；

#### **3. `$.extend` 的作用和用法?**
>`$.extend` 是把将两个或更多对象的内容合并到 **第一个** 对象。

- `$.extend( target [, object1 ] [, objectN ] )`
***target***：一个对象，如果附加的对象被传递给这个方法将那么它将接收新的属性，如果它是唯一的参数将扩展jQuery的命名空间；
***object1***：待合并到第一个对象的对象；
***objectN***：待合并到第一个对象的对象；

- `$.extend( [deep], target [, object1 ] [, objectN ] )`
***deep***：如果是 `true`，合并成为递归（又叫做深拷贝）。例如下面一个例子：

```javascript
var student01 = {name:"licao",age:20};
var student02 = {name:"xiaomin",age:25,sex:"man"};
$.extend(student01,student02);
```

运行后`student01`将变为 `{age:25,name:"xiaomin",sex:"man"}`,而`student02`不变；

如果我们想保留原对象，我们可以通过传递一个空对象作为目标对象：

```javascript
var student01 = {name:"licao",age:20};
var student02 = {name:"xiaomin",age:25,sex:"man"};
var object = $.extend({}, object1, object2);
```
运行后`object`将变为 `{age:25,name:"xiaomin",sex:"man"}`，而`student01` , `student02`不变；

#### **4. JQuery 的链式调用是什么？**
链式调用是一种语法招数，通过多次重复使用同一个变量来达到用少量代码表达复杂操作的目的，代码看起来更加优雅。缺点是占用了函数的返回值。当我们在实现一个`hover`效果：

![](http://7xr868.com1.z0.glb.clouddn.com/%E9%93%BE%E5%BC%8F%E8%B0%83%E7%94%A8.gif)
`mouseover`时显示半透明层，`mouseleave`时不显示，这其实就是 jQuery 链式调用的反映：
```javascript
$(".panel").on('mouseover', '.item', function(){
    $(this).find('.item-hover').show();
}).on('mouseleave', '.item', function(){
    $(this).find('.item-hover').hide();
});
```
我们选中的对象 `'.item'` 执行完一个方法后就返回本身（`return this`）,然后被返回的对象继续执行后面的方法，可以写一下 [小例子][2] 增加理解


#### **5. JQuery Ajax 中缓存怎样控制?**
首先得了解为什么我们二次访问后会访问之前一次请求成功后的缓存？这是浏览器的一种机制，当使用 Ajax 请求回来数据以后，浏览器会将请求的 URL 和数据缓存起来，然后当我们第二次请求时，浏览器先匹配本次URL是与之前留在缓存里的URL一致，是则给你本地缓存的数据，不会请求web服务器。
>如何解决 JQuery Ajax 缓存问题

- 调用 `$.ajaxSetup ({cache:false})` 方法

-  


#### **6. jQuery 中 `data` 函数的作用**
在匹配元素上存储任意相关数据 或 返回匹配的元素集合中的第一个元素的给定名称的数据存储的值。

`.data()` 方法允许我们在DOM元素上绑定任意类型的数据,避免了循环引用的内存泄漏风险。

- `.data( key, value )`：在匹配元素上存储任意相关数据
- `.data( key )`：返回匹配的元素集合中的第一个元素的给定名称的数据存储的值

```javascript
$('body').data("data",20);
```
```javascript
$('body').data("data");//20
$("body").data();//[object Object] { data: 20 }
```
我们可以在 dom 元素中存值然后取回

---
>### 【练习】

#### **1. 写出以下功能对应的 jQuery 方法：**

1. 给元素 `$node` 添加 class `active`，给元素 $noed 删除 class `active`
```javascript
$node.addClass("active");//添加 class

$node.removeClass("active");//删除 class
```

2. 展示元素 `$node`, 隐藏元素`$node`
```javascript
$node.show();//展示元素 `$node`
$node.fadeIn("slow");//通过淡入的方式显示元素。

$node.hide();//隐藏元素`$node`
$node.fadeOut("slow");//通过淡入的方式隐藏元素。
```

3. 获取元素 `$node` 的 属性: id、src、title， 修改以上属性
```javascript
$node.attr("id");
$node.attr("src");
$node.attr("title");

//单独设置一个简单属性
$node.attr("id","yourid");

//一次设置多个属性
$node.attr({
  id: "yourid",
  src: "http://www.licao404.com/...",
  title: "jQuery"
});
```

4. 给 `$node` 添加自定义属性 `data-src`
```javascript
$node.attr("data-src","value");
```

5. 在 `$ct` 内部最开头添加元素 `$node`
```javascript
$ct.prepand($node);
```

6. 在 `$ct` 内部最末尾添加元素 `$node`
```javascript
$ct.append($node);
```
7. 删除 `$node`
```javascript
$node.remove(); //同时移除移除元素内部的一切,包括元素上的事件及 jQuery 数据。
$node.detach(); //删除的元素不删除数据和事件
```

8. 把 `$ct` 里内容清空
```javascript
$ct.empty();//为了避免内存泄漏，jQuery先移除子元素的数据和事件处理函数，然后移除子元素。
```

9. 在 `$ct` 里设置 html `<div class="btn"></div>`
```javascript
$ct.html('<div class="btn"></div>');
```

10. 获取、设置 `$node` 的宽度、高度(分别不包括内边距、包括内边距、包括边框、包括外边距)
```javascript
$node.height()//获取匹配元素集合中的第一个元素的当前计算高度值(不包括padding)。
$node.height(200)//设置高度为200px,不输入单位默认是 px

$node.innerHeight()//获得匹配集合中第一个元素的当前计算的内部高度（包括padding，但不包括border）
$node.innerHeight(200)//

$node.outerHeight()//获得匹配集合中第一个元素的当前计算的高度（）
$node.outerHeight(false)///获得匹配集合中第一个元素的当前计算的高度（包括border，但不包括margin）

$node.outerHeight(true)///获得匹配集合中第一个元素的当前计算的高度（包括margin）

//宽度设置也是和高度设置一样；
```

11. 获取窗口滚动条垂直滚动距离
```javascript
$node.scrollTop();
```

12. 获取 `$node` 到根节点水平、垂直偏移距离
```javascript
$node.offset();
```

13. 修改 `$node` 的样式，字体颜色设置红色，字体大小设置14px
```javascript
$node.css({
  "color":"red",
  "font-size":"14px"
});
```

14. 遍历节点，把每个节点里面的文本内容重复一遍
```javascript
$node.each(function(){
  $(this).clone.insertAfter($(this));
});
```

15. 从 `$ct` 里查找 class 为 `.item` 的子元素
```javascript
$ct.find('.item');
```

16. 获取 `$ct` 里面的所有孩子
```javascript
$ct.chidren()
```

17. 对于 `$node`，向上找到 class 为’.ct’的父亲，在从该父亲找到’.panel’的孩子
```javascript
$node.parent('.ct').find('.panel');
```

18. 获取选择元素的数量
```javascript
$node.length;
```

19. 获取当前元素在兄弟中的排行
```javascript
$node.index();
```

#### **2. 简单实现以下操作：**
1. 当点击 `$btn` 时，让 `$btn` 的背景色变为红色再变为蓝色
```javascript
$btn.on('click',function(){
  $(this).animate({backgroundColor:'red'},1000).animate({backgroundColor:'blue'},1000);
});
```

2. 当窗口滚动时，获取垂直滚动距离
>[demo][3]
```javascript
<script>
  $(window).on('scroll',function () {
    console.log( $(document).scrollTop() )
  });
</script>
```

3. 当鼠标放置到 `$div` 上，把`$div` 背景色改为红色，移出鼠标背景色变为白色
>[demo][4]
```javascript
$button.on("mouseenter", function() {
      $(this).css("background-color", "red");
  }).on("mouseleave", function() {
      $(this).css("background-color", "white");
  });
```

4. 当鼠标激活 `input` 输入框时让输入框边框变为蓝色，当输入框内容改变时把输入框里的文字小写变为大写，当输入框失去焦点时去掉边框蓝色，控制台展示输入框里的文字
>[demo][5]
```javascript
$ipt.on('focusin',function(){
  $(this).css('border','1px solid blue');
}).on('keyup',function(){
  $(this).val($(this).val().toUpperCase());
}).on('focusout',function(){
  $(this).css('border','');
  console.log($(this).val());
});
```

5. 当选择 `select` 后，获取用户选择的内容
>[demo][6]
```javascript
$select.on('change',function(){
  var selected = $(this).val();
  console.log(selected);
});
```

#### **3. 用 jquery ajax 实现如下效果。`当点击加载更多会加载数据展示到页面。当鼠标放置上去会变色`**
>[示例效果][7]

```php
<?php
    // 后端 php 测试接口文件
    $start = $_GET['start']; //2
    $len = $_GET['len'];  //6
    $items = array();

    for($i = 0; $i < $len; $i++){
        array_push($items, '内容' . ($start+$i));
    }
    $ret = array('status'=>1, 'data'=>$items);

    //{status: 1, data: ['内容1','内容2','内容3']}
    sleep(0.5);
    echo json_encode($ret);
```

> [效果点点我][7]
[瞅瞅代码][8]


---

[1]:http://2.jrgapp.applinzi.com/%E4%BD%9C%E4%B8%9A%E5%AE%89%E6%8E%92/jscode/JS9-jqueryajax/1.html
[2]:http://www.imooc.com/code/3402
[3]:http://js.jirengu.com/lic/2/edit
[4]:http://js.jirengu.com/lipe/1/edit?output
[5]:http://js.jirengu.com/kal/2/edit?html,console,output
[6]:http://js.jirengu.com/poc/1/edit?console,output
[7]:http://febox.applinzi.com/task26/task26-3.html
[8]:https://github.com/licao404/landemo/blob/master/task26/task26-3.html
