---
title: 'DOM,事件'
categories:
  - 编程学习
  - 前端基础
tags:
  - JavaScript
date: 2016-04-03 13:13:41
---
<blockquote class = "blockquote-center">DOM（文档对象模型）是针对HTML、XML和SVG文档的API（编程接口）。DOM将前述的结构化文档解析成一个层次化的节点树（DOM Tree）。所有的节点和树状结构都有规范的API，这样我们就可以通过编程语言操作（增删改查）这些文档。严格地说，DOM不属于JavaScript，但是操作DOM是JavaScript最常见的任务，而JavaScript也是最常用于DOM操作的语言。所以，DOM往往放在JavaScript里面介绍。下文是利用JavaScript对DOM标准的实现。</blockquote>
<!--more-->

>### 【知识点】

#### **1. `dom对象`的`innerText`和`innerHTML`有什么区别？**
- `innerHTML`获取的是这个元素节点包含的所有HTML代码，包括标签，文本内容，例如：

```html
<body>
    <div id="p1">我是段落1
		<a class="link" href="#">FE</a>
	</div>
</body>

<script>
    var p1 = document.getElementById('p1');
    console.log( p1.innerHTML);
    //输出：
    //我是段落1
    // <a class="link" href="#">FE</a>
</script>
```
- `innerText`自动忽略当前节点内部的HTML标签，获取的是这个元素节点包含的所有文本字符串内容，进行拼接。输出结果为：

```html
<script>
    console.log( p1.innerText);
    //输出：
    //我是段落1 FE
</script>
```
- `innerHTML`是符合W3C标准的属性，而`innerText`只适用于IE浏览器；


- `innerText`会自动对HTML标签转义，什么意思呢，继续修改上面的例子：p标签解释为文本，即`&lt;p&gt;`，而不会当作标签处理，正因为如此，`innerText`比`innerHTML`更加安全，可以防止`XSS攻击`;

```html
<script>
    p1.innerText = "<p>hello</p>";
    console.log( p1.innerHTML); //&lt;p&gt;hello&lt;/p&gt;
    console.log( p1.innerText); //<p>hello</p>
</script>
```


#### **2. elem.children和elem.childNodes的区别？**
- `children`属性是非标准的，返回一个类似数组的动态对象（实时反映变化），包括当前元素节点的所有子元素集合。如果当前元素没有子元素，则返回的对象包含零个成员。

- `childNodes`属性是标准的，返回一个包括当前节点的所有子节点的`NodeList`集合，包括文本节点Text，Comment节点。


#### **3. 查询元素有几种常见的方法？**
1.  `getElementById(id)`：返回指定ID属性的元素节点.找不到匹配的节点，返回`null`；特别的，在搜索匹配节点时，id属性是大小写敏感的。比如，如果某个节点的id属性是`main`，那么`document.getElementById("Main")`将返回`null`，而不是指定节点。

2. `getElementByClassName(class)`：该方法返回一个类数组对象，包括了所有class名字符合指定条件的元素（搜索范围包括本身），元素的变化实时反映在返回结果中。这个方法不仅可以在document对象上调用，也可以在任何元素节点上调用。

3. `getElementsByTagName()`：返回所有指定标签的元素。返回值是一个`HTMLCollection`对象，也就是说，搜索结果是一个动态集合，任何元素的变化都会实时反映在返回的集合中。这个方法不仅可以在`document`对象上调用，也可以在任何元素节点上调用。

4. `getElementsByName()`：返回拥有`name属性`的HTML元素，比如`form`、`img`、`frame`、`embed和object`，返回一个`NodeList`格式的对象，不会实时反映元素的变化。

5. `querySelector()`：返回匹配指定的`CSS选择器`的元素节点。如果有多个节点满足匹配条件，则返回`第一个匹配`的节点。如果没有发现匹配的节点，则返回`null`。
```javascript
var el2 = document.querySelector('#myParent > [ng-click]');
```
    其中`querySelector()`无法选中CSS伪元素

6. `querySelectorAll()`：返回匹配指定的`CSS选择器`的所有节点，返回的是`NodeList类型`的对象。NodeList对象不是动态集合，所以元素节点的变化无法实时反映在返回结果中。

#### **4. 如何创建一个元素？如何给元素设置属性？**
1. - 使用`createElement()`方法来创建一个`HTML`元素节点，例如创建一个`img`元素：
```javascript
var img = document.createElement('img');
```

 - 使用`setAttribute()`方法来**设置元素属性**，例如：
```javascript
img.setAttribute("src","1.png");
```

 - 然后使用`appendChild()`，把元素节点添加到指定元素节点末尾，比如添加到`body`元素节点末尾：
```javascript
document.body.appendChild(img);
```

2. `createTextNode()`方法创建文本节点：
```javascript
var text = document.createTextNode('hello!');
document.body.appendChild(text);
```
    这个方法可以确保返回的节点，被浏览器当作txt文本渲染，而不是当作HTML代码（恶意代码会被转义）渲染。因此，可以用来展示用户的输入，避免`XSS攻击`

3. `createDocumentFragment()`生成一个`DocumentFragment`对象。



#### **5. 元素的添加、删除？**
- 使用`appendChild()`添加元素到指定元素的末尾：
```javascript
document.body.appendChild(document.createElement('img'));
```

- 使用`insertBefore()`添加元素到指定元素之前;

- 使用`removeChild()`删除元素;
```javascript
body.removeChild(document.getElementById('p1'));
```

#### **6. `DOM0` 事件和 `DOM2级` 在事件监听使用方式上有什么区别？**
- `DOM0`级事件处理程序只能给元素绑定一个事件，例如给`button`绑定两个`onclick`事件：

```html
<body>
    <div id="">
        <button type="button" name="button" id="btn"></button>
    </div>
</body>

<script>
    document.getElementById("btn").onclick = function(){
        console.log("hello");
    }
    document.getElementById("btn").onclick = function(){
        console.log("world");
    }
</script>
```

点击`button`只会打印出`world`,这种方式绑定的事件只会在时间流的冒泡阶段被处理

- `DOM2`级事件处理程序可以绑定多个事件（新浏览器处理方式），修改上面的例子：
```html
<script>
    document.getElementById("btn").addEventListener("click",function(){console.log("helo");},false);
    document.getElementById("btn").addEventListener("click",function(){console.log("world");},false);
</script>
```

点击`button`会先后打印出`hello`、`world`，通过`addEventListener()`绑定的事件处理程序只能通过`removeElementListener()`移除。`addEventListener()`第三个参数为布尔值，设为`false`时表示在事件流的冒泡阶段触发事件，`true`则为捕获阶段。

```javascript
document.getElementById("p1").addEventListener("click",function(){console.log("helo parents");},true);

document.getElementById("btn").addEventListener("click",function(){console.log("helo");},true);
```

上面的事件在事件捕获阶段被触发，绑定在父元素上的事件先于目标事件被触发，先后输出`hello parents`、`hello`；

#### **7. `attachEvent`与`addEventListener`的区别？**
1. 参数不同：
 - 参数个数不同，`addEventListener()`有3个参数，`attachEvent()`有2个参数；`attachEvent()`事件处理程序只能绑定在事件冒泡阶段，`addEventListener()`可以通过其第3个参数设定为`true`和`false`来决定事件处理程序绑定在冒泡还是捕获阶段；（一般情况下都是冒泡，出于兼容性考虑）

 - 第一个参数意义不同，`addEventListener()`第一个参数是事件类型（`click` ，`load`），`attachEvent()`第一个参数是事件处理函数名称（`onclick`，`onload`）

2. 事件处理程序的作用域不同。`addEventListener()`作用域是元素本身，`this`触发元素；`attachEvent()`在全局作用域中运行，`this`等于`window`;

3. 时间处理程序的执行顺序不同。`addEventListener()`是以添加它们的顺序执行，`attachEvent()`执行顺序一般是则相反的；

#### **8. 解释`IE`事件冒泡和`DOM2`事件传播机制？**
- IE事件冒泡：事件开始由具体的元素接收，然后一层层往上传播到较不具体的节点，基本上所有的浏览器都支持事件冒泡；

- `DOM2级`事件传播包含3个阶段，事件捕获阶段 ——> 处于目标阶段 ——> 事件冒泡阶段，首先是事件捕获，从最上层节点一层层往下传播，然后目标节点接收事件，最后进行事件冒泡；



#### **9. 如何阻止事件冒泡？ 如何阻止默认事件？**
- `stopPropagation()`方法用于立即停止事件在DOM层次中的传播，阻止事件进一步冒泡或捕获，例如：

```html
<body>
    <button id="btn">Click Me</button>
</body>

<script>
    document.querySelector('#btn').onclick = function(e){
        console.log(e);
        console.log("hello");
        e.stopPropagation();
        console.log(this.innerText());
        console.log(e.target);
    }

    document.body.onclick = function(e){
        console.log(e);
        console.log("world!");
        console.log(e.target);
    }
</script>
```

添加了`stopPropagation()`方法阻止了事件冒泡到`body`,绑定在`body`上的事件没有被触发。

- 对IE来讲，是通过将`cancelBubble`属性值设置为`true`来取消冒泡：

```javascript
    document.getElementById('btn').attachEvent("onclick",function(e){
        e.cancelBubble = true;
    })
```


- `preventDefault()`方法用于取消事件默认行为,例如点击`a链接`会发生跳转是默认行为，可以通过`e.preventDefault()`阻止这一行为：

```html
<body>
    <a href="http://www.baidu.com" class="btn">Click Me</a>
</body>

<script>
    document.querySelector('.btn').addEventListener('click',function(e){
        console.log("hello");
        e.preventDefault();
    })
</script>
```

- 对IE来讲，是通过将`returnValue`属性值设置为`false`来取消默认行为：
```javascript
document.getElementById('btn').attachEvent("onclick",function(e){
    e.returnValue = false;
})
```

---
>### 【练习】

#### **1. 有如下代码，要求当点击每一个元素li时控制台展示该元素的文本内容。不考虑兼容**

```html
<ul class="ct">
    <li>让我们</li>
    <li>一起学习</li>
    <li>JavaScript</li>
</ul>

<script>
//使用事件代理：
    document.querySelector('.ct').onclick = function(e){
        console.log(e.target.innerText);
    }
</script>
```
或者采用原始方法：
```html
<script>
//给每个<li>标签绑定事件
    var arr = document.getElementsByTagName('li');
    for(var i = 0;i < arr.length;i++){
    	arr[i].addEventListener('click', function () {
    		 console.log(this.innerText);
    	})
    }    
</script>
```


#### **2. 补全代码，要求如下：**
1. 当点击按钮开头添加时在`<li>让我们</li>`元素前添加一个新元素，内容为用户输入的非空字符串；当点击结尾添加时在`<li>JavaScript</li>`后添加用户输入的非空字符串;
2. 当点击每一个元素`li`时控制台展示该元素的文本内容;    

 - [效果点我](http://febox.applinzi.com/task-22/task-22-2.html)

 - 代码如下：(事件代理的一种[错误写法](http://jsbin.com/relowi/7/edit?html,console,output))

```html
<ul class="ct">
    <li>让我们</li>
    <li>一起学习</li>
    <li>JavaScript</li>
</ul>
<input class="ipt-add-content" placeholder="添加内容"/>
<button id="btn-add-start">开头添加</button>
<button id="btn-add-end">结尾添加</button>
<script>
var ul = document.querySelector('.ct'),
    addEnd = document.querySelector('#btn-add-end'),
    addStart = document.querySelector('#btn-add-start');

// ul.onclick = function (e) {
//      console.log(e.target.innerText) ;
// }//注意：这样写会出问题，当点击到的是ul而不是li时（ul有padding时）会出错
//改为以下这种写法：
ul.addEventListener('click',function(e){
    if (e.target.tagName.toLowerCase() === 'li') {
        console.log(e.target.innerText);
    }
});

addEnd.onclick = function () {
    var item = document.createElement('li');
    item.innerText = document.querySelector('.ipt-add-content').value;
    if (item.innerText) {
        ul.appendChild(item);
        document.querySelector('.ipt-add-content').value = null;
    }else {
        alert("Input Nothing!");
    }
}

addStart.onclick = function (){
    var item = document.createElement('li');
    item.innerText = document.querySelector('.ipt-add-content').value;
    if (item.innerText) {
        ul.insertBefore(item,ul.firstChild);
        document.querySelector('.ipt-add-content').value = null;
    }else {
        alert("Input Nothing!");
    }
}
</script>
```

#### **3. 补全代码，要求：当鼠标放置在`li`元素上，会在`img-preview`里展示当前`li`元素的`data-img`对应的图片。**
- [效果点我](http://febox.applinzi.com/task-22/task-22-3.html)

- 代码如下：

```html
<ul class="ct">
    <li data-img="http://7xrvo9.com1.z0.glb.clouddn.com/0320/2016-03-20%2012.19.57%201.jpg">鼠标放置查看图片1</li>
    <li data-img="http://7xrvo9.com1.z0.glb.clouddn.com/0320/2016-03-20%2012.24.13%201.jpg">鼠标放置查看图片2</li>
    <li data-img="http://7xrvo9.com1.z0.glb.clouddn.com/0320/2016-03-20%2012.28.55%201.jpg">鼠标放置查看图片3</li>
</ul>
<div class="img-preview"></div>
<script>
var ct = document.querySelector('.ct'),
    preview = document.querySelector('.img-preview');

ct.addEventListener('mouseover', function (e) {
    preview.innerHTML = '<img src="'+e.target.getAttribute('data-img')+'" style="width:500px">';
});
</script>
```

#### **4. 实现`tab`切换功能**
- [效果点我点我](http://febox.applinzi.com/task-22/task-22-4.html)
- [瞅瞅代码](https://github.com/licao404/landemo/blob/master/task-22/task-22-4.html)

#### **5. 实现模态框功能**
- [效果点我点我](http://febox.applinzi.com/task-22/task-22-5.html)
- [瞅瞅代码](https://github.com/licao404/landemo/blob/master/task-22/task-22-5.html)









---
