---
title: OOP For JavaScript
date: 2016-05-10 22:08:53
categories: [编程学习,前端基础]
tags: [JavaScript,OOP]
---
>### 【知识点】

#### **1. OOP 指什么？有哪些特性 **

OOP （Object Oriented Programming）是面向对象编程，是一种计算机编程架构。面向对象语言（C++，Java等）有一个标志，都有类的概念，通过类可以创建多个具有相同属性和方法的对象（类的实例化）。OOP 达到了软件工程的三个主要目标：重用性、灵活性和扩展性。为了实现整体运算，每个对象都能够接收信息、处理数据和向其它对象发送信息。

<!--more-->

OOP的特征：
- **封装**：确保组件（数据和功能一起在运行着的计算机程序中形成的单元）不会以不可预期的方式改变其它组件的内部状态；只有在那些提供了内部状态改变方法的组件中，才可以访问其内部状态。每类组件都提供了一个与其它组件联系的接口，并规定了其它组件进行调用的方法。

- **抽象**：程序有能力忽略正在处理中信息的某些方面，即对信息主要方面关注的能力。

- **多态**：组件的引用和类集会涉及到其它许多不同类型的组件，而且引用组件所产生的结果依据实际调用的类型。

- **继承**：允许在现存的组件基础上创建子类组件，这统一并增强了多态性和封装性。典型地来说就是用类来对组件进行分组，而且还可以定义新类为现存的类的扩展，这样就可以将类组织成树形或网状结构，这体现了动作的通用性。


#### **2. 如何通过构造函数的方式创建一个拥有属性和方法的对象?**

创建自定义的构造函数，从而定义自定义对象类型的属性和方法，例如下面的构造函数模式：
```javascript
function Student(name,age,score){
    this.name = name;
    this.age = age;
    this.score = score;
    this.alertScore = function(){
        alert(this.name + ':' + this.score);
    };
}
```
创建一个 `Student` 的实例，需要使用 **new** 操作符：
```javascript
var xiaomin = new Student("xiaomin",20,80);
var gardon = new Student("gardon",22,75);
```
用 `new` 调用构造函数实际上经过了4中步骤：
- 创建一个新对象；
- 将构造函数的作用域赋给新对象(this指向这个新对象)；
- 执行构造函数中的代码，将属性和方法添加到这个对象上；
- 返回新对象

#### **3. prototype 是什么？有什么特性**

JavaScript中创建的每一个函数都有一个 **prototype（原型）** 属性，这个属性是一个指针，指向一个原型对象，这个对象拥有一系列属性和方法且能被所有特定类型的实例共享。换而言之 `prototype` 就是上题中通过调用构造函数创建的那个对象实例(`xiaomin`)的原型对象。

特性：让特定对象的所有实例共享 `prototype` (原型对象)包含的属性和方法。不用在构造函数中定义对象实例的信息，而是可以将这些信息直接添加到 `prototype` 中,如上题中：
```javascript
function Student(){};
Student.prototype.name = "John";
Student.prototype.age = 18;
Student.prototype.score = 90;
Student.prototype.alertScore = function(){
    alert(this.name + ':' + this.score);
}
var xiaomin = new Student();//alert John:90
console.log(xiaomin.age);//console.log 18
var gardon = new Student();
gardon.alertScore();
console.log(xiaomin.alertScore === gardon.alertScore);//true
```

#### **4. 画出如下代码的原型图**
```javascript
function People (name){
  this.name = name;
  this.sayName = function(){
    console.log('my name is:' + this.name);
  }
}

People.prototype.walk = function(){
  console.log(this.name + ' is walking');  
}

var p1 = new People('小明');
var p2 = new People('蓝岚');
```
![](http://7xrvo9.com1.z0.glb.clouddn.com/2016.0511/%E5%8E%9F%E5%9E%8B%E5%9B%BE2.png)



#### **5. 以下代码中的变量 name 有什么区别**
```javascript
function People (){
  var name = "小明"            //定义了一个局部变量name
  this.name = "我";             //给this所指的对象（多种情况）绑定一个name属性
}
People.name = "jscode";         //给函数People（对象）绑定一个name属性

People.prototype.name = "蓝岚";//给People的原型对象绑定一个name属性，如果new一个People的实例，这个实例的name属性为“我”，而不是原型对象上的“学前端”  
```


---

>### 【练习】

#### **1. 创建一个 Car 对象，拥有属性name、color、status；拥有方法run，stop，getStatus**
```javascript
function Car(name,color,status){
    this.name = name;
    this.color = color;
    this.status = status;
    this.run = function(){
        this.status = "runing";
    };
    this.stop = function(){
        this.status = "stop";
    };
    this.getStatus = function(){
        return this.status;
    };
}
```



#### **2. 创建一个 GoTop 对象，当 new 一个 GotTop 对象则会在页面上创建一个回到顶部的元素，点击页面滚动到顶部。拥有以下属性和方法**
- ct 属性，GoTop 对应的 DOM 元素的容器
- target属性， GoTop 对应的 DOM 元素
- bindEvent 方法， 用于绑定事件
- createNode 方法， 用于在容器内创建节点

```javascript
function GoTop($ct){
    this.ct = $ct;
    this.target = $('<div id="back-top">Top</div>');
}
GoTop.prototype.createNode = function(){
    this.ct.append(this.target);
};
GoTop.prototype.bindEvent = function(){
    this.target.on('click',function(){
        $(window).scrollTop(0);
    });
};
```


#### **3. 使用构造函数创建对象的方式完成轮播功能( demo )，使用如下调用方式**
```javascript
function Carousel($node){
//todo...
}
Carousel.prototype = {
//todo ..
};

var $node1 = $('.ct').eq(0);
var $node2 = $('.ct').eq(1);
var carousel1 = new Carousel($node1);
var carousel2 = new Carousel($node2);
```

- [效果预览](http://febox.applinzi.com/task35/task35-1.html)

- [实现代码](https://github.com/licao404/landemo/blob/master/task35/task35-1.html)

#### **4. 使用构造函数创建对象的方式实现 Tab 切换功能**

- [效果预览](http://febox.applinzi.com/task35/task35-2.html)

- [实现代码](https://github.com/licao404/landemo/blob/master/task35/task35-2.html)



---
