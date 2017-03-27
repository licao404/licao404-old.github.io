---
title: 原型链后续之JavaScript的继承
categories:
  - 编程学习
  - 前端基础
tags:
  - JavaScript
date: 2016-05-14 12:15:51
---
![](http://7xr868.com1.z0.glb.clouddn.com/task36/Person%E5%8E%9F%E5%9E%8B%E5%9B%BE.png)

<blockquote class="blockquote-center">还记得这张原型图么（没见过 or 忘了的请戳[《OOP For JavaScript》](./2016/05/10/OOP-For-JavaScript/)和[《说清楚JavaScript的原型链》](./2016/05/12/原型链/)这两篇博文）？深入学习了JavaScript原型和原型链的的概念和原理后，我们内心会隐隐有种道不明的感觉，如果你学习过其他面向对象语言，你会不自觉发现这是种多么熟悉的感觉啊——这不就像是继承么？此文就将内心的感觉整理出来，作为JavaScript语言的一个核心概念来学习。</blockquote>

<!--more-->

>### 【理论知识】

#### **继承有什么作用? **
- 继承是面向对象语言中一个重要的概念，通过继承实现代码复用，拓展软件功能；
- 不像其他面向对象语言的继承有着诸如“父类”和“子类”的概念，父类的的属性和方法子类可以继承而不必重写，只需要写出新增或者改写的内容。JavaScript是基于对象的语言，没有类的概念，所以，要想实现继承，就需要用js的原型链或者用 `apply` 和 `call` 方法。


#### **有几种常见创建对象的方式? 举例说明?**
- 工厂模式 ，由于JavaScript中无法创建类，所以通过一个函数来封装以特定接口创建对象的细节，例如：
```javascript
function creatHuman(name,age){
    var o = {
        name: name,
        age: age,
        selfIntro: function(){
            console.log("I'm " + name);
        }   
    };
    return o;
}
var people1 = creatHuman("Gardon",20);
var people2 = creatHuman("John",18);
```

- 构造函数模式 ，自定义对象类型的属性和方法：
```javascript
function Human(name,age){
        this.name = name,
        this.age = age,
        this.selfIntro = function(){
            console.log("I'm " + name);
    };
}
var people1 = new Human("Gardon",20);
var people2 = new Human("John",18);
```

- 原型模式
```javascript
function Human(){};
Human.prototype.name = "Gardon",
Human.prototype.age = 20,
Human.prototype.selfIntro = function(){
    console.log("I'm " + this.name);
};
var people1 = new Human();
var people2 = new Human();
```

- 构造函数模式与原型模式结合 ， 目前JavaScript中使用最广泛，认同度最高的一种创建自定义类型的方法：
```javascript
function Human(name,age){
        this.name = name,
        this.age = age
}
Human.prototype.selfIntro = function(){
    console.log("I'm " + this.name);
};
var people1 = new Human("Gardon",20);
var people2 = new Human("John",18);
```

- ......



#### **下面两种写法有什么区别?**
```javascript
//方法1
function People(name, sex){
    this.name = name;
    this.sex = sex;
    this.printName = function(){
        console.log(this.name);
    }
}
var p1 = new People('Gardon', 20)

//方法2
function Person(name, sex){
    this.name = name;
    this.sex = sex;
}

Person.prototype.printName = function(){
    console.log(this.name);
}
var p1 = new Person('John', 18);
```
经过前面的总结，可知：上面的代码采用两种不同的创建对象的模式。
- 方法1 用的是构造函数模式，每次通过 `new` 创建 `People` 的新实例，会为每个新实例绑定属性和方法，也就是每个 `People` 的实例都被添加了 `printName` 方法；

- 方法2 构造函数模式与原型模式结合，实例的属性都是在构造函数中定义的，而由所有实例共享的属性和方法则是在原型中定义的，也就是所有 `Person` 的实例都继承了 `Person` 原型对象的 `printName` 方法，由原型链指向也可发现这种区别。


#### **Object.create 有什么作用？兼容性如何？如何使用？**
ECMAScript5 通过新增 `Object.create()` 方法创建一个拥有指定原型和若干个指定属性的对象，规范化了原型式继承；
这个方法接受两个参数，第一个是用作新对象原型的对象、第二个是一个为新对象定义额外属性的对象，是个可选值；

>好吧，这样解释确实有点晦涩，继续来个 `Human` 的栗子加个原型图说明下：

```javascript
//这里首先新建了个构造函数Human（对象）
function Human(name,age){
    this.name = name;
    this.age = age;
}
Human.prototype.selfIntro = function(){
    console.log("I'm " + this.name);
};
//这里又新建了个构造函数GreatMan（对象）
function GreatMan(trait){
    this.trait = trait;
    Human.call(this);//重新定义Human函数的运行上下文为当前GreatMan的上下文
}
```
现在需要 `GreatMan` 继承自 `Human` 。除了原型链继承，我们使用一种原型式的继承：将 `Human` 的原型拷贝一份，这用到了 `Object.create()` 方法，然后让新对象（也就是 `GreatMan`） 的 `prototype` 指向拷贝来的原型：
```javascript
GreatMan.prototype = Object.create(Human.prototype);

var turing = new GreatMan("diligent");
console.log(turing.selfIntro());//I'm undefined

turing.name = "Turing";
console.log(turing.selfIntro());//I'm Turing
```

>原型图示：

![](http://7xr868.com1.z0.glb.clouddn.com/task37/%E5%8E%9F%E5%9E%8B%E5%9B%BEcreate.png)

>考虑兼容性：支持 Object.create() 方法的浏览器有 `IE9+`、`Firefix4+`、`Safari5+`、`Opera12+` 和 `Chrome`  

#### **hasOwnProperty 有什么作用？ 如何使用？**
`hasOwnProperty()` 方法用来判断某个对象是否含有指定的自身属性，参数是要检测的是属性名。
所有继承了 `Object.prototype` 的对象都会从原型链上继承到 `hasOwnProperty` 方法，这个方法可以用来检测一个对象是否含有特定的自身属性，如对上面的例子:
```javascript
function Human(name,age){
    this.name = name;
    this.age = age;
    this.selfAge = function(){
        console.log("I'm " + this.age);    
    };
}
Human.prototype.selfIntro = function(){    
    console.log("I'm " + this.name);
};
var people1 = new Human("Gardon",20);

people1.hasOwnProperty('name');//true
people1.hasOwnProperty('age');//true
people1.hasOwnProperty('selfAge');//true
people1.hasOwnProperty('selfIntro')//false
```
我们创建 Human 的实例 `people1`，people1 的属性包含 name、ag、selfAge，所以 `hasOwnProperty` 返回的是 `true`,而 `selfIntro` 是其指向的原型的属性，所以 `hasOwnProperty` 返回 `false`。


#### **实现 Object.create 的 polyfill，如：（ps: 写个 函数create，实现 Object.create 的功能）**   
>[什么是 polyfill？](http://www.cnblogs.com/ziyunfei/archive/2012/09/17/2688829.html)

`Object.create` 的内部实现逻辑前面的解释和原型图已经很明了，现在通过自定义函数实现：

```javascript
//create传入的参数是需要继承的对象
function create(obj){
    function F(){}//基于传入对象创建一个临时的构造函数
    F.prototype = obj;//将传入的对象作为构造函数的原型
    return new F();//返回构造函数的一个新实例
}
var obj = {a: 1, b:2};
var obj2 = create(obj);
console.log(obj2.a); //1
```
区区3行,这是 ECMAScript 另外一种继承的思路：原型式继承。最初由 [Douglas Crockford](http://www.crockford.com/) 在其文章 [《Prototypal Inheritance in JavaScript》](http://javascript.crockford.com/prototypal.html) 中提出。

ECMAScript5 通过新增 `Object.create()` 方法创建一个拥有指定原型和若干个指定属性的对象，规范化了原型式继承，`Object.create()` 在传入一个参数的情况下，和 `create()` 方法的行为相同。


#### **如下代码中 call 的作用是什么?**
```javascript
function Person(name, sex){
    this.name = name;
    this.sex = sex;
}
function Male(name, sex, age){
    Person.call(this, name, sex);    //这里的 call 有什么作用
    this.age = age;
}
```
前面谈论 `this` 时（参见[《关于 "this" 二三事》](./2016/05/04/关于-this-二三事/#more)）已经简单提过 `call` 和 `apply` 函数的一般作用：用来动态改变某个函数运行时上下文（重新定义函数的执行环境），也就是函数内部 `this` 的指向。

`call` 和 `apply` 也正是通过这种作用来实现继承，上面代码中，`call` 将 `Person` 这个构造函数的运行时上下文变为 `Male` 这个构造函数（`Person` 内部的 `this` 指向 `Male`）;

这时候通过 `new` 新建 `Male` 的实例对象：
```javascript
var goodman = new Male("Gardon","male",20);

console.log(goodman.age);//20
console.log(goodman.name);//Gardon
console.log(goodman.sex);//male
```

发现 `goodman` 是不是就有了 `name` 和 `sex` 属性呢？然而 `Male` 中并没有这两个属性，而是通过 `call` 函数继承了 `Person` 的属性。

>apply 函数与 call 函数作用相同，参数传入方式略有区别，同样参见之前的博文[《关于 "this" 二三事》](./2016/05/04/关于-this-二三事/#more)
```javascript   
Person.apply(this,[name, sex]);
```



#### **补全代码，实现继承**
```javascript
function Person(name, sex){
    this.name = name;
    this.sex = sex;
}

Person.prototype.getName = function(){
    return this.name;
};    

function Male(name, sex, age){
   this.age = age;  
   Person.call(this,name,sex)//call方法继承属性
}

Male.prototype = Object.create(Person.prototype);//Object.create继承原型方法

Male.prototype.printName = function(){
    console.log(this.getName())
};
Male.prototype.getAge = function(){
    return this.age;
};

var john = new Male('Gardon', 'male', 20);
john.printName();//Gardon
```

---

>### 【小试牛刀】

#### **实现如下可拖拽 dialog 弹窗功能，如下是功能要求**

```javascript
//功能描述：
// 1. 可使用 dialog.open() 去打开弹窗
// 2. 当点击确定、取消时可使用用户自定义事件
// 3. dialog 可拖动
// 4. 允许页面展示多个 dialog


function Dialog(){
//todo ...
}


var tpl = '<ul><li>列表1</li><li>列表2</li><li>列表1</li><li>列表1</li></ul>';

$('#open4').on('click',function(){
  var dialog4 = new Dialog();
  dialog4.open({
    title: 'Message',
    message: tpl,
    isShowCloseBtn: true,
    isShowConfirmBtn: true,
    onClose: function(){
      alert('取消')
    },
    onConfirm: function(){
      alert('确定');
    }
  });
});

```

首先实现一个普通的 `dialog`，前面用过原生 JavaScript 实现过，但是还比较简陋。现在我们用面向对象的写法将其封装成jQuery插件：
>- [DEMO预览](http://febox.applinzi.com/goDialog/index.html)

>- [实现代码](https://github.com/licao404/landemo/tree/master/goDialog)

然后我们在上面的基础上，加入拖拽事件，
>- [DEMO预览](http://febox.applinzi.com/task37/index.html)

>- [实现代码](https://github.com/licao404/landemo/tree/master/goDialogdraggable)




---
