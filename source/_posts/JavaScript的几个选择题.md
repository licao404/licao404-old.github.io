---
title: JavaScript的几个选择题
date: 2016-05-28 17:31:07
categories:
    - 前端基础
    - 编程学习
tags:
    - JavaScript
---
<blockquote class="blockquote-center">Not a lot really. Quiz mainly focuses on knowledge of scoping, function expressions (and how they differ from function declarations), references, process of variable and function declaration, order of evaluation, and a couple more things like `delete` operator and object instantiation. These are all relatively simple concepts, which I think every professional Javascript developer should know. Most of these are applied in practice quite often. Ideally, even if you can't answer a question, you should be able to infer answer from specs (without executing the snippet). When creating these questions, I made sure I can answer each one of them off the top of my head, to keep things relatively simple.
Note, however, that not all questions are very practical, so don't worry if you can't answer some of them. We don't often use `with` statement, for example, so failing to know/remember its exact behavior is understandable.
</blockquote>

<!--more-->

## Few notes about code
- Assuming ECMAScript 3rd edition (not 5th)
- Implementation quirks do not count (assuming standard behavior only)
- Every snippet is run as a global code (not as eval or function one)
- There are no other variables declared (and host environment is not extended with anything beyond what's defined in specs)
- Answer should correspond to exact return value of entire expression/statement (or last line)
- "Error" in answer indicates that overall snippet results in a runtime error


## Now Begin

### 1

```javascript
(function(){
  return typeof arguments;
})();
```
- "object"
- "array"
- "arguments"
- "undefined"



### 2

```javascript
var f = function g(){ return 23; };
typeof g();
```
- "number"
- "undefined"
- "function"
- Error


### 3

```javascript
(function(x){
  delete x;
  return x;
})(1);
```
- 1
- null
- undefined
- Error


### 4

```javascript
var y = 1, x = y = typeof x;
x;
```
- 1
- "number"
- undefined
- "undefined"


### 5

```javascript
(function f(f){
  return typeof f();
})(function(){ return 1; });
```
- "number"
- "undefined"
- "function"
- Error


### 6

```javascript
var foo = {
  bar: function() { return this.baz; },
  baz: 1
};
(function(){
  return typeof arguments[0]();
})(foo.bar);
```
- "undefined"
- "object"
- "number"
- "function"


### 7

```javascript
var foo = {
  bar: function(){ return this.baz; },
  baz: 1
}
typeof (f = foo.bar)();
```
- "undefined"
- "object"
- "number"
- "function"


### 8

```javascript
var f = (function f(){ return "1"; }, function g(){ return 2; })();
typeof f;
```
- "string"
- "number"
- "function"
- "undefined"


### 9

```javascript
var x = 1;
if (function f(){}) {
  x += typeof f;
}
x;
```
- 1
- "1function"
- "1undefined"
- NaN


### 10

```javascript
var x = [typeof x, typeof y][1];
typeof typeof x;
```
- "number"
- "string"
- "undefined"
- "object"



### 11

```javascript
(function(foo){
  return typeof foo.bar;
})({ foo: { bar: 1 } });
```
- "undefined"
- "object"
- "number"
- Error



### 12

```javascript
(function f(){
  function f(){ return 1; }
  return f();
  function f(){ return 2; }
})();
```
- 1
- 2
- Error (e.g. "Too much recursion")
- undefined


### 13

```javascript
function f(){ return f; }
new f() instanceof f;
```
- true
- false


### 14

```javascript
with (function(x, undefined){}) length;
```
- 1
- 2
- undefined
- Error

---
