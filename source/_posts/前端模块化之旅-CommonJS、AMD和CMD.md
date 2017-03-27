title: 前端模块化之旅（二）：CommonJS、AMD和CMD
date: 2016-05-18 11:36:24
categories:
  - 编程学习
  - 前端进阶
tags:
  - JavaScript
  - 模块化
---
{% fullimage http://7xrvo9.com1.z0.glb.clouddn.com/20160518/%E5%89%8D%E7%AB%AF%E6%A8%A1%E5%9D%97%E5%8C%96%E4%B9%8B%E6%97%85%E4%BA%8C.jpg %}

<blockquote class="blockquote-center">继续前篇，各种模块化规范开始推出，其中比较突出的是服务器端的 CommonJS 规范,它是 Nood.JS 在实践中推出的，也是首先采用 JS 模块化概念的语言，跳出了浏览器；进而出现了浏览器环境的模块化方案 AMD和CMD。</blockquote>

<!--more-->

>#### CommonJS Modules/1.0

CommonJS 规范是服务器端的模块化的规范，是 Nood.js 在实践中推出的，Nood.js 也是首先采用 js 模块化的；

它规定一个单独的文件就是一个模块，一个模块中存在一个自由变量 **require**，这是个函数，用于加载模块：
- 这个 `require` 函数接受一个模块标识符，返回外部模块所输出的 `API`;
- 如果出现[依赖闭环](http://weizhifeng.net/commonjs-module-1.0-specification.html)(dependency cycle)，那么外部模块在被它的传递依赖（transitive dependencies）所 `require` 的时候可能并没有执行完成；在这种情况下，"require"返回的对象必须至少包含此外部模块在调用require函数（会进入当前模块执行环境）之前就已经准备完毕的输出。
- 如果请求的模块不能返回，那么"require"必须抛出一个错误。

在一个模块中，会存在一个名为 **exports** 的自由变量，这是一个对象，模块可以在执行的时候把自身的API加入到其中，用于定义模块，导出给其他地方使用；

`exports` 对象是输出模块变量的唯一方式。

参照下面的一个例子：
```javascript
//math.js
exports.add = function(a,b){
    var c = a + b;
    return c;
}

//index.js
var add = require('math').add;//
console.log(add(1,1));//2
```

在 `math.js`中将 add 函数绑定到模块中的 `exports` 对象中，之后在 `index.js` 模块中用 `require` 方法加载了 `math.js` 模块，并调用该模块中的 add 函数。


>#### AMD

Asynchronous Module Definition，即异步的模块定义，是浏览器端的模块化规范，是 RequireJS 在推广过程中对模块定义的规范化产出。

与服务器端的模块化规范 CommonJS 不同，AMD 的模块加载是异步的，因为是浏览器端，所以势必要是异步的（浏览器同步加载模块会导致性能、可用性、调试和跨域访问等问题）。因为模块异步加载时不会影响后面程序的执行，前面总结过 js 异步的情况，依赖某些模块的语句均放置在回调函数中，等待模块加载完成后再执行；

AMD 规范只定义了一个函数 **define** ，是一个全局变量，如下定义一个模块的语法：
```javascript
define(id?, dependencies?, factory);
```
- `id`：模块的名字，如果没有提供该参数，模块的名字应该默认为模块加载器请求的指定脚本的名字；

- `dependencies`：模块的依赖，已被模块定义的模块标识的数组字面量。依赖参数是可选的，如果忽略此参数，它应该默认为 `["require", "exports", "module"]`。然而，如果工厂方法的长度属性小于3，加载器会选择以函数的长度属性指定的参数个数调用工厂方法。

- `factory`：模块的工厂函数，模块初始化要执行的函数或对象。如果为函数，它应该只被执行一次。如果是对象，此对象应该为模块的输出值。

参照下面的一个例子：
```javascript
define('myModule',['jQuery','types/Employee'],function($,Employee){//定义模块myModule，引入依赖jQuery，types/Employee
	 function Programmer(){
            //do something
        };
        Programmer.prototype = new Employee();
        return Programmer;  //return Constructor
})
```

>#### CMD

Common Module Definition，即通用模块定义，是浏览器端的模块化规范，是 SeaJS 在推广过程中对模块定义的规范化产出。

如下定义一个模块的语法:
```javascript
define(factory)
```
- `factory` 为函数时，表示是模块的构造方法。执行该构造方法，可以得到模块向外提供的接口。factory 方法在执行时，默认会传入三个参数：require、exports 和 module.

>AMD 是**依赖关系前置，提前执行**；CMD 是类似于 CommonJS 那样 **按需加载，延迟执行**：

```javascript
//CMD recommanded
define(function(require, exports, module){
	var a = require('a');
	a.doSomething();
	var b = require('b');
	b.doSomething();	// 依赖就近，延迟执行
});

//AMD recommanded
define(['a', 'b'], function(a, b){
    a.doSomething();    // 依赖前置，提前执行
    b.doSomething();
});
```
明显看出和 AMD 不同，模块定义时已不用立马引入依赖，而是运行到需要时候再加载，根据顺序执行，这样更像是 CommonJS 的风格，让人感觉也像是同步加载似的。但实际上 CMD 内部处理是对文件做了一个词法的解析，在还没执行的时候，解析出所需的依赖，并不是真正的同步。

---

>参考：

- [CommonJS Modules/1.0 规范](http://weizhifeng.net/commonjs-module-1.0-specification.html)
- [AMD 模块定义规范](https://github.com/amdjs/amdjs-api/wiki/AMD-(%E4%B8%AD%E6%96%87%E7%89%88)
- [CMD 模块定义规范](https://github.com/seajs/seajs/issues/242)






















---
