---
title: JavaScript基本语法要点
date: 2016-03-16 10:56:36
categories: [编程学习,前端基础]
tags: JavaScript
---
{% gp 3-3 %}
![](http://7xrvo9.com1.z0.glb.clouddn.com/0316%2Fportfolio-1.jpg)
![](http://7xrvo9.com1.z0.glb.clouddn.com/0316%2Fportfolio-2.jpg)
![](http://7xrvo9.com1.z0.glb.clouddn.com/0316%2Fportfolio-3.jpg)
{% endgp %}
<!--more-->
>### 【基础知识点】

#### **1. CSS和JS在网页中的放置顺序是怎样的？**
- 一般而言`CSS`放在`<head>`头部而`JS`放在`<body>`底部。

#### **2. 解释白屏和`FOUC`？**
* `白屏`：
    * 当样式表放在页面底部时，一些浏览器（IE浏览器）会等待加载完html和css后才进行页面渲染，这样就造成页面内容不会逐步出现，而是有空白等待的时间；注意`@import`加载样式时即使放在`link`里也会造成白屏；
    * `JS`是阻塞加载，放在头部时会禁止并发加载，等js加载完成后再继续加载，也会造成白屏；

* `FOUC`（无样式内容闪烁）是因为另外一些浏览器每解析一个标签就渲染一步，如果样式表放在后面，会在最后加载完成css后再进行一次渲染，造成闪烁,`Firfox`是一直`Fouc`;


#### **3. async和defer的作用是什么？有什么区别？**
* 作用：都是让`JS`的加载和后续文档元素的加载渲染并行执行（异步）；
* 区别:`defer`只是异步加载而不会立即执行,等待页面后续内容全部加载完成后才执行；`async`则是加载`js`后立即执行;

#### **4. 简述网页的渲染机制？**
1. 浏览器根据用户输入的域名去获取对应的网页资源；
2. 解析`html`标签构建`DOM`树;
3. 解析`CSS` 标签构建`CSSOM`树;
4. 把`DOM`和`CSSOM`组合成为渲染树;
5. 精确计算每个DOM节点的位置（`layout`和`reflow`过程）；
6. 最后通过调用操作系统`Native GUI`的`API`绘制;
7. 更加详细深入借鉴[知乎][1]；

#### **5. `JavaScript` 定义了几种数据类型? 哪些是简单类型?哪些是复杂类型?**
> `JavaScript`定义了 6 种数据类型

* 简单类型
    * `NULL`空数据类型
    * `Undefined`未定义的数据类型
    * `String`字符串类型：由0个或多个字符组成，被包含在引号里，字符包括字母，数字，标点符号和空格；
    * `Number`数值类型：支持整数，浮点数，负数，
    * `布尔值`：`true`/`false`
* 复杂类型：
    * `Object`对象

#### **6. `NaN`、`undefined`、`null`分别代表什么?**
* `NaN`:`NaN`属性顾名思义`not a number`，表示非数字值，并不是合法的数字；但是数据类型是`Number`;不等于任何值包括自己；
* `undefined`：表示未定义的值；`typeof`一个没有值得变量会返回`undefined`;
* `null`：空数据类型，表示一个空对象的引用，只有一个值的特殊类型;`typeof`检测`null`返回`object`;

#### **7. `typeof`和`instanceof`的作用和区别?**
* `typeof`可以用来检测`Function`，`Number`，`Undefined`，`String`等这几种基本类型，但对于数组和正则只能返回`object`;
* `instanceof`用来判断某个对象的实例，返回布尔值

---
>### 【基本语法演示】

#### **1.完成如下代码判断一个变量是否是数字、字符串、布尔、函数**

``` javascript
function isNumber(el){
    if (typeof(el)==="number") {
    	return true;
    }
}
function isString(el){
    if (typeof(el)==="string") {
    	return true;
    }
    return false;
}
function isBoolean(el){
    if (typeof(el)==="boolean") {
    	return true;
    }
    return false;
}
function isFunction(el){
    if (typeof(el)==="function") {
    	return true;
    }
}
var a = 2,
    b = "jirengu",
	c = false;
	alert( isNumber(a) );  //true
	alert( isString(a) );  //false
	alert( isString(b) );  //true
	alert( isBoolean(c) ); //true
	alert( isFunction(a)); //false
	alert( isFunction( isNumber ) ); //true		
```

#### **2.以下代码的输出结果是?**

```javascript
console.log(1+1);			//2
console.log("2"+"4");		//24 两个字符串连接
console.log(2+"4"); 		//24 2转换为字符
console.log(+new Date());	//1458035114537
console.log(+"4");			//4
```

#### **3.以下代码的输出结果是?**

```javascript
var a = 1;
a+++a;

typeof a+2;//number2

```

#### **4.遍历数组，把数组里的打印数组每一项的平方?**

```javascript
var arr = [3,4,5]
for(var i=0;i<arr.length;i++){
	console.log(arr[i]*arr[i]);
}
// 输出 9, 16, 25
```

#### **5.遍历 `JSON`, 打印里面的值?**

```javascript
var obj = {
  name: 'hunger',
  sex: 'male',
  age: 28
}
for(var k in obj){
	console.log(k+": "+obj[k]);
}
// 输出 name: hunger, sex: male, age:28
```


#### **6.下面代码的输出是? 为什么?**

```javascript
console.log(a);//Undefined,声明了变量并进行了变量提升，未有赋值
var a = 1;
console.log(a);//1
console.log(b);//报错，b没有被声明
```


  [1]: http://www.zhihu.com/question/20117417

---
