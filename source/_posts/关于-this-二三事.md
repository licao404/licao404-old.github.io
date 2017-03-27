---
title: 关于 "this" 二三事
date: 2016-05-04 19:30:55
categories: [编程学习,前端基础]
tags: JavaScript
---
>### 【知识点】

#### ** apply、call 有什么作用，什么区别？**
- `call` 和 `apply` 都是 `Function.prototype` 的方法，即每个 `Function` 对象的实例（function定义的每个方法）都有这两个方法。
>二者的作用完全一样，都是用来**动态改变某个函数运行时上下文**（重新定义函数的执行环境），也就是函数内部 `this` 的指向。

<!--more-->

- `call` 和 `apply` 的区别仅仅是在参数定义的方式不太一样,具体表现为：
>`call` 和 `apply` 方法的第一个参数都是要传入给当前对象的对象，或者是 `this` ;后面的参数都是传递给当前对象的参数，`call` 需要把参数按照顺序放进去，而 `apply` 则是把参数放在数组里;

- 如下面的一个小例子，体现了这两个函数的共同作用：改变函数内部 `this` 的指向。又可以看到细微差别，变量传入方式的不同。
```javascript
var animal = {
	name: 'tiger',
	food: 'meat',
	fn: function (name,food) {
		 this.name = name; 
		 this.food = food;
		 console.log(this);
	}
};

fn.call(animal,'sheep','grass'); //Object {name: "sheep", food: "grass"}
fn.apply(animal,['panda','bamboo']); //Object {name: "panda", food: "bamboo"}
```

---
>### 【练习】

#### **1. 以下代码输出什么?**

```javascript
var john = { 
  firstName: "John" 
}
function func() { 
  alert(this.firstName + ": hi!")
}
john.sayHi = func
john.sayHi() //alert出 John:hi!

```
- 输出：`John:hi!`
- 解析：`john.sayHi()` 这样写就是对象 `john` 调用了函数 `func()`（或者说将函数 `func()` 绑定在对象 `john` 上）。函数 `func()` 内部的 `this` 指向的就是对象 `john`。


#### **2. 下面代码输出什么，为什么**

```javascript
func() 

function func() { 
  alert(this)
}
```
- 输出：`Winsow`对象
- 解析：直接 `func()` 这样写就是 `Winsow` 对象调用了函数 `func()`（或者说此时函数 `func()` 的执行环境是在全局环境下）。函数 `func()` 内部的 `this` 指向的就是对象 `Winsow`。


#### **3. 以下代码输出什么?**

```javascript
function fn0(){
    function fn(){
        console.log(this);
    }
    fn();
}

fn0();
```
- 输出：`Winsow` 对象
- 解析：追本溯源还是 `Winsow` 对象调用了 `fn()`;


```javascript
document.addEventListener('click', function(e){
    console.log(this);
    setTimeout(function(){
        console.log(this);
    }, 200);
}, false);
```
- 输出：`document` 文档对象 , `Winsow` 对象
- 解析：因为clik事件绑定在 document 上，`this` 指向 document 文档对象;延时器的执行环境都是全局的，`this` 指向 window


#### **4. 下面代码输出什么，为什么**

```javascript
obj = {
  go: function() { alert(this) }
}

obj.go(); //输出 obj 对象，obj 对象调用了函数 go();

(obj.go)(); //输出 obj 对象，立即执行函数写法，还是 obj 对象调用了函数 go();

(a = obj.go)(); //输出 Window 对象，a 是一个全局变量，函数赋值给一个全局变量，a() 执行时依赖的就是全局环境;

(0 || obj.go)(); //输出 Window 对象，(0 || obj.go) 就是 (obj.go)，所以(obj.go)()，调用它的仍然是全局对象;
```

#### **5. 下面代码输出什么，为什么**

```javascript
var john = { 
  firstName: "John" 
}

function func() { 
  alert( this.firstName )
}
func.call(john) 
```
- 输出：`John` 
- 解析：`call` 函数的用法：只有一个参数，指定 `func()` 函数内部 `this` 指向 `john` 对象.


#### **6. 下面代码输出什么，为什么**

```javascript
var john = { 
  firstName: "John",
  surname: "Smith"
}

function func(a, b) { 
  alert( this[a] + ' ' + this[b] )
}
func.call(john, 'firstName', 'surname') 
```
- 输出：`John Smith` 
- 解析：`call` 函数的用法：第一个参数指定 `func()` 函数内部 `this` 指向 `john` 对象,后面的参数按顺序传入。


#### **7. 以下代码有什么问题，如何修改**

```javascript
var module= {
  var self = this;  //4. 这里先保存 this 的值，使其指向对象 module
  bind: function(){
    $btn.on('click', function(){
      console.log(this) //1. this指向的是 $btn ,因为 $btn 绑定了事件处理函数
      this.showMsg(); //2. 因为 this 指向的是 $btn, $btn上怎么会有showMsg()呢，显然只有 module 上有
      //3. 所以这里需要在之前保存下this值，改为
      self.showMsg();
    })
  },
  
  showMsg: function(){
    console.log('饥人谷');
  }
}
```