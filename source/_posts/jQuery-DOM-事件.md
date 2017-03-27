---
title: 'jQuery DOM & 事件'
categories:
  - 编程学习
  - 前端基础
tags:
  - JavaScript
  - jQuery
date: 2016-04-16 09:51:23
---
>### 【知识点】

#### **1. 说说库和框架的区别？**
- **库** 是把一些可以复用的方法集中进行封装到一个`library`，提供`API`，我们通过这些`API`就可以调用这些已经封装好的方法，将代码写得更少/巧/强壮，以便简单快捷完成任务，提高生产力！比如 `jQuery` 就是更加接近一个库，要什么可以从中拿；
- **框架** 也算是库的一种，不过更加强调提供一种解决方案，给你搭好架子，代码怎样组织，所以不单单是具体的方法或函数，而是注重从细节到整体的宏观思路。`AngularJS` 就算是一个框架，因为它提供了一整套的解决方案；

<!--more-->
>对于小型应用来说，因为业务逻辑简单，代码总量不会太大，组织不会是太大的问题，所以用类库就够了；而面对中/大型项目，特别是需要多人共同参与的项目，选择一个合适的框架有助于写出规范的，易于理解的，易于回复的，低耦合的……等等的代码，在此基础上再使用各种类库来增加具体代码的健壮性与功能性则更好。

#### **2. jQuery 能做什么？**
一言以蔽之，jQuery就是让我们能更加畅快舒爽地编写JavaScript，不必去编写蛋疼费时的原生JavaScript！它轻量，快速，功能丰富。
- 可以简单快捷完成诸如JavaScript中的HTML元素选取和操作，CSS操作，HTML `DOM`遍历和操作，事件处理，特效，动画和`AJAX`交互。
- jQuery提供`API`让开发者编写插件。其模块化的使用方式使开发者可以很轻松的开发出功能强大的静态或动态网页。

#### **3. jQuery 对象和 DOM 原生对象有什么区别？如何转化？**
- DOM对象是通过原生JavaScript获得的对象，jQuery对象是通过jQuery选择器获得对象；
- jQuery对象只能使用jQuery自己的属性和方法，DOM对象只能使用DOM的属性和方法，

>jQuery对象转DOM对象

我们可以通过类数组下标的获取方式或者get方法获取指定下标的`DOM`对象:
```html
<div id="ct">
    <div class="item">item1</div>
    <div class="item">item2</div>
    <div class="item">item3</div>
</div>
```
jQuery 对象是类数组对象，所以可通过数组下标获取数组中的DOM对象：
```javascript
$('.item')[2];//<div class="item">item3</div>
$('.item').eq(2)[0];//<div class="item">item3</div>
```
通过jQuery的`.get()`方法获得的直接是DOM对象：
```javascript
$('.item').get(2);//<div class="item">item3</div>
```

>DOM对象转 jQuery 对象

使用 `$()` 将DOM对象包裹起来就是jQuery对象：
```javascript
var ct = document.getElementById('ct');//DOM对象
var $ct = $(ct);//$(DOM对象)转化为了jQuery对象$ct
```


#### **4. jQuery中如何绑定事件？`bind`、`unbind`、`delegate`、`live`、`on`、`off`都有什么作用？推荐使用哪种？使用`on`绑定事件使用事件代理的写法？**
jQuery中有很多绑定事件的方法`.bind()`,`.live()`,`.delegate()`,`.on()`,下面看看他们的区别：
- `.bind()`：对于早期jQuery版本（1.7以前），`.bind()`方法用于直接将事件绑定在选中的元素上，所以`.bind()`在绑定事件的时候被选的元素必须存在；使用一次`.bind()`选择器匹配的元素会附加一个事件处理函数，而以后再添加的元素则不会有。为此需要再使用一次 `.bind()` 才行；

- `.unbind`：`.bind()`的反向操作，从每一个匹配的元素中删除绑定的事件。
如果没有参数，则删除所有绑定的事件，`.bind(typpe)`删除指定类型事件；

- `.live()`：（1.9版本已废弃,不建议使用，老版本建议使用`.delegate()`）默认使用了事件代理（绑定在祖先元素上的事件处理程序可以对后代上触发事件作出回应），没有将事件直接绑定在该元素上，而是绑定在`document`元素上（DOM树的根节点），
```html
<body>
  <div class="clickme">Click here</div>
</body>

<script>
$('.clickme').live('click', function() {
      alert("Live handler called."); //事件冒泡`document`元素上，查看事件类型是否是click、
      //目标元素和选择器.clickme否匹配，
      //是则执行绑定的事件处理函数；
});
</script>
```

- `.delegate()`：对于早期jQuery版本（1.7以前），`.delegate`是使用事件代理最有效的方式,
```html
<div style="background-color:red">
<p>这是一个段落。</p>
<button>请点击这里</button>
</div>

<script>
$('div').delegate('button','click',function(){
    $('p').slideToggle();//将事件绑定到$('div')上，事件冒泡到$('div')上
})//查看是不是click事件，目标元素是不是button，如果都满足，就执行函数
</script>
```

- `.on()`：（1.7版本后`.on()`取代了`.bind()`,`.live()`,`.delegate()`，提供绑定事件处理的所有功能，**强烈推荐**）
事件代理
```javascript
$("body").on("click", "p", function(){
    alert( $(this).text() );//当点击段落时，显示该段落中的文本
});javascript
```

- `.off()`：移除用`.on()`绑定的事件处理程序。


#### **5. jQuery 如何展示/隐藏元素？**
- **.hide( speed, [callback] )**：隐藏显示的元素，
`speed`：表示速度的参数，可选（`slow`,`fast`,`normal`）或者毫秒数;
`[callback]`:在动画完成时执行的函数，每个元素执行一次。
不设置参数就立即执行，没有动画；

- **.show(speed,callback)**：显示隐藏的元素；


#### **6. jQuery 动画如何使用？**
**.animate(params,[speed],[callback])** 函数用来创建自定义动画，这个函数的关键在于指定动画形式及结果样式属性对象。这个对象中每个属性都表示一个可以变化的样式属性（如“height”、“top”或“opacity”）。注意：所有指定的属性必须用骆驼形式，比如用marginLeft代替margin-left.
```html
<style>
    .box{
        width: 50px;
        height:50px;
        background-color: red;
    }
    .btn{
      margin-bottom:10px;
    }
</style>

<body>
    <button class="btn">Click</button>
    <div class="box"></div>
</body>

<script>
  $('.btn').on('click',function repeat(){
    $('.box').animate({
      width:"100px",
      height:"100px",
      opacity:0.5
    },1000,function(){
      $(this).animate({
        width:"50px",
        height:"50px",
        opacity:1
      },1000,repeat());
    });
  });
</script>
```
效果：
![][4]


#### **7. 如何设置和获取元素内部 HTML 内容？如何设置和获取元素内部文本？**
- **`$(selector).html()`**用于获取**第一个**匹配元素的内容，。这个函数不能用于XML文档，但可以用于XHTML文档。如果选择器匹配多个元素，那么只有第一个匹配元素的 HTML 内容会被获取；

- **`$(selector).html( htmlString )`**用于设定每一个匹配元素的html内容;也可以通过函数来设置html内容：
```javascript
$('div.demo-container').html(function() {
  var emph = '<em>' + $('p').length + ' paragraphs!</em>';
  return '<p>All new content for ' + emph + '</p>';
}); 
```

- **`$(selector).text()`**取得所有匹配元素的文本内容。这个方法对HTML和XML文档都有效。注意：`.text()` 方法不能使用在 `input` 元素或`scripts`元素上。 `input` 或 `textarea` 需要使用 **`.val()`** 方法获取或设置文本值。得到`scripts`元素的值，使用`.html()`方法

- **`$(selecor).text( textString )`**用于设置每一个匹配元素的文本内容；也可以通过函数来设置：
```javascript
$("p").text(function(n){
    return "这个 p 元素的 index 是：" + n;
    });
```

#### **8. 如何设置和获取表单用户输入或者选择的内容？如何设置和获取元素属性？**
- **`$(selector).val()`**获取匹配元素的当前（输入/选择）值，如果多选，将返回一个数组，其包含所选的值。例如：如果是`select`元素， 当没有选择项被选中，它返回`null`;如果`select`元素有`multiple`（多选）属性，并且至少一个选择项被选中，`.val()`方法返回一个数组，这个数组包含每个选中选择项的值；

- **`$(selector).val(value)`**设置匹配元素输入或选择的值；也可以通过函数设定文本框的值；

>例如将文本框输入变为大写：

```html
<body>
  <input class="ipt" type="text" name="ipt">
</body>

<script>
    $('.ipt').on('keyup',function(){
      $(this).val($(this).val().toUpperCase());
    });
</script>
```

- **`.attr(name|properties|key,value|fn)`**获取匹配的元素集合中的**第一个**元素的属性的值  或 设置每一个匹配元素的一个或多个属性。
```javascript
//  设置一个属性
$('#img1').attr('alt', 'rain');

//设置多个属性
$('#img1').attr({
  alt: 'rain',
  title: 'cloud'
});

//设置函数返回
$("img").attr("title", function() { return this.src });
```

---
>### 【练习】

#### **1. 使用 jQuery实现如下效果**

![][1]

- **[效果点我][7]**
- **[瞅瞅代码][8]**


#### **2. 使用 jQuery实现如下效果**

![][2]

>**问题：** 点奢侈品2的时候页面跳到了顶部，可能是什么原因？如何解决

- **[效果点我][9]**
- **[瞅瞅代码][10]**



#### **3. 使用 jQuery实现如下效果**

![][3]

- **[效果点我][11]**
- **[瞅瞅代码][12]**

>**提示**

1.使用代理
2.当点击按钮时使用如下数据
```javascript
var products = [
    {
        img: 'http://img10.360buyimg.com/N3/jfs/t2242/92/1446546284/374195/9196ac66/56af0958N1a723458.jpg',
        name: '珂兰 黄金手 猴哥款',
        price: '￥405.00'
    },{
        img: 'http://img10.360buyimg.com/N3/jfs/t2242/92/1446546284/374195/9196ac66/56af0958N1a723458.jpg',
        name: '珂兰 黄金转运珠 猴哥款',
        price: '￥100.00'
    },{
        img: 'http://img10.360buyimg.com/N3/jfs/t2242/92/1446546284/374195/9196ac66/56af0958N1a723458.jpg',
        name: '珂兰 黄金手链 3D猴哥款',
        price: '￥45.00'
    }
];
```
---

>参考：

- [jQuery API 1.12 中文文档][5]
- [jQuery API 中文文档][6]

---






[1]:http://7xnk1s.com2.z0.glb.qiniucdn.com/8-1.gif
[2]:http://7xnk1s.com2.z0.glb.qiniucdn.com/8-2.gif
[3]:http://7xnk1s.com2.z0.glb.qiniucdn.com/8-3-1.gif
[4]:http://7xr868.com1.z0.glb.clouddn.com/task25/animate%28%29.gif
[5]:http://jquery.cuishifeng.cn/
[6]:http://www.css88.com/jqapi-1.9/
[7]:http://febox.applinzi.com/task25/task25-1.html
[8]:http://js.jirengu.com/vor/8/edit?html
[9]:http://febox.applinzi.com/task25/task25-2.html
[10]:http://js.jirengu.com/doj/1/edit?html
[11]:http://febox.applinzi.com/task25/task25-3.html
[12]:http://js.jirengu.com/duh/3/edit?html