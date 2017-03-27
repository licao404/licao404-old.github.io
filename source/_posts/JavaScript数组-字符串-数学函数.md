---
title: 数组/字符串/数学函数
date: 2016-03-19 17:08:17
categories: [编程学习,前端基础]
tags: JavaScript
---
>### 【知识点】

#### **1. 数组方法里`push`、`pop`、`shift`、`unshift`、`join`、`split`分别是什么作用？**
<!--more-->

```javascript
//结合push()和pop()方法让JavaScript数组实现了类似栈的操作，后进先出：
//push():用于向数组的队尾压入任意数量的参数，返回压入后的数组长度
var arr = [1,2,3,4];
console.log(arr.push(5,'abc'),arr);
//输出结果应为 6 [1, 2, 3, 4, 5, "abc"]

//pop():用于弹出数组的最后一项，返回该项，数组长度减一
console.log(arr.pop()); //应该返回abc,数组此时为[1,2,3,4,5]


//shift():压出数组的第一项，返回该项，数组长度减一
console.log(arr.shift()); //应该返回1，数组此时为[2,3,4,5]
//结合push()和shift()方法可以实现类似数据结构中的队列操作，后进后出

//unshift():用于向数组的队首压入任意数量的参数，返回压入后的数组长度
console.log(arr.unshift('abc',1)，arr); //输出结果为 6 ["abc", 1, 2, 3, 4, 5]
//同理，unshift()结合pop()可以实现队列操作，只是反方向而已

//join(separator):用于把数组中的所有元素连接成一个字符串。separator是连接的符号，可选，默认是逗号
arr.join() //结果是"abc,1,2,3,4,5
arr.join('')//结果是"abc12345"

//split(separator,howmany):用于把一个字符串分割成字符串数组,separator取值为字符串或正则表达式，
//从这个字符串的地方分割，返回被分割了的字符串组成数组（不包括separator本身）。howmany可选，可指定
//返回的数组的最大长度。如果设置了该参数，返回的子串不会多于这个参数指定的数组。如果没有设置该参数，整
//个字符串都会被分割，不考虑它的长度。
```
---
>### 【练习】


>#### 数组

#### **1.用 `splice` 实现 `push`、`pop`、`shift`、`unshift`方法？**

```javascript
var arr = [2,3,3,4];
console.log('arr[]=',arr);
var arrpop = arr.splice(arr.length-1,1);//splice(arr.length-1,1)实现pop()
console.log('arrpop后arr[]=', arr);
console.log('arrpop返回：',arrpop);//splice()

arr.push(4);
var arrpush = arr.splice(arr.length,0,5);//splice(arr.length,0,xx)实现push()
console.log('arrpush后arr[]=', arr)

arr.pop();
var arrshift = arr.splice(0,1);//实现shift()
console.log('arrshift后arr[]=', arr);
console.log('arrshift返回：',arrshift)

arr.unshift(2);
var arrunshift = arr.splice(0,0,5);//实现unshift()
console.log('arrunshift后arr[]=', arr);
```

#### **2.使用数组拼接出如下字符串：**

```javascript
function getTplStr(data){
	var htmls = [];
	htmls.push('<dl class="product">');
	htmls.push('	<dt>'+prod.name+'</dt>');
	for (var i = 0; i < prod.styles.length; i++) {
		htmls.push('	<dd>'+prod.styles[i]+'</dd>');
	}
	htmls.push('</dl>')
	return htmls.join('');
};
var result = getTplStr(prod);  
```
```html
<dl class="product">
    <dt>女装</dt>
    <dd>短款</dd>
    <dd>冬季</dd>
    <dd>春装</dd>
</dl>
```

#### **3.写一个`find`函数，实现下面的功能:**

```javascript
var arr = [ "test", 2, 1.5, false ]
function find(arr,x){
			console.log(arr.indexOf(x));//使用indexOf()方法返回给定元素能找在数组中找到的第一个索引值，否则返回-1。
		}
find(arr, "test") // 0
find(arr, 2) // 1
find(arr, 0) // -1
```


#### **4.写一个函数`filterNumeric`，实现如下功能**

```javascript
arr = ["a", 1, "b", 2];
arr = filterNumeric(arr);  //   [1,2]

//****************函数如下**********************
function filterNumeric(arr){
		function filterNumeric(arr){
		console.log(arr.filter(function(x){return typeof x === "number"}));
		}
```


#### **5.对象obj有个className属性，里面的值为的是空格分割的字符串(和html元素的class特性类似)，写`addClass`、`removeClass`函数，有如下功能：**

```javascript
var obj = {
  className: 'open menu'
}
addClass(obj, 'new') // obj.className='open menu new'
addClass(obj, 'open')  // 因为open已经存在，此操作无任何办法
addClass(obj, 'me') // obj.className='open menu new me'
console.log(obj.className)  // "open menu new me"

removeClass(obj, 'open') // obj.className='menu new me'
removeClass(obj, 'blabla')  // 不变

//****************addClass函数如下************************
function addClass(obj,str){
	var arr = obj.className.split(' ');
	if (arr.indexOf(str) == -1) {
		arr.push(str);
	}
	obj.className = arr.join(' ');
}

//****************removeClass函数如下**********************
function removeClass(obj,str){
	var arr = obj.className.split(' ');
	if (arr.indexOf(str) != -1) {
		arr.splice(arr.indexOf(str),1);
	}
	obj.className = arr.join(' ');
}
```


#### **6.写一个`camelize`函数，把my-short-string形式的字符串转化成myShortString形式的字符串，如：**

```javascript
console.log(camelize("background-color"));  //== 'backgroundColor'
console.log(camelize("list-style-image"));  //== 'listStyleImage'

//****************函数如下**********************
function camelize(str){
	var x = str.split('-');
	var sum = x[0];
	for(var i=1;i<x.length;i++){
		sum += x[i].slice(0,1).toUpperCase()+x[i].slice(1);
	}
	return sum;
}
```

#### **7.下面代码的输出是? 为什么?**

```javascript
arr = ["a", "b"];
arr.push( function() { alert(this) } );//push将一个匿名函数压从尾部入数组arr
//数组arr此时为["a","b",function anonymous()]
arr[arr.length-1]()//将[arr.length-1]位置上的functio()变为立即执行函数，
//alert(this)输出当前数组，结果是a,b,function(){alert(this)}
```

#### **8.写一个函数`filterNumericInPlace`，过滤数组中的数字，删除非数字**

```javascript
arr = ["a", 1, "b", 2];
filterNumericInPlace(arr);
console.log(arr)  // [1,2]

//****************函数如下**********************
function filterNumericInPlace(arr){
	for (var i = 0; i < arr.length; i++) {
		if (typeof(arr[i] != "number")) {
			arr.splice(i, 1);
		}
	}
}
```

#### **9.写一个`ageSort`函数实现如下功能：**

```javascript
var john = { name: "John Smith", age: 23 }
var mary = { name: "Mary Key", age: 18 }
var bob = { name: "Bob-small", age: 6 }
var people = [ john, mary, bob ]
ageSort(people) // [ bob, mary, john ]

//****************函数如下**********************
function ageSort (arr) {
	return arr.sort(function(a,b){
		return b - a;
	})
}
```

#### **10.写一个`filter(arr, func)` 函数用于过滤数组，接受两个参数，第一个是要处理的数组，第二个参数是回调函数(回调函数遍历接受每一个数组元素，当函数返回true时保留该元素，否则删除该元素)。实现如下功能：**

```javascript
function isNumeric (el){
    return typeof el === 'number';
}
arr = ["a", -1, 2, "b"]

arr = filter(arr, isNumeric) ; // arr = [-1, 2],  过滤出数字
arr = filter(arr, function(val) { return val > 0 });  // arr = [2] 过滤出大于0的整数

//****************函数如下**********************
function filter(arr,func){
			return arr.filter(func);
		}

```

>#### 字符串

#### **1.写一个`ucFirst`函数，返回第一个字母为大写的字符**

```javascript
//ucFirst("gardon") == "Gardon"
function ucFirst (str) {
	 return str.slice(0,1).toUpperCase() + str.slice(1);
}
console.log(ucFirst("gardon"));
```

#### **2.写一个函数`truncate(str, maxlength)`, 如果str的长度大于maxlength，会把str截断到maxlength长，并加上...，如：**

```javascript
// truncate("hello, this is gardon valley,", 10) == "hello, thi...";
// truncate("hello world", 20) == "hello world"

//****************函数如下**********************
function truncate (str,maxlength) {
	 if(str.length > maxlength){
		return(str.slice(0,maxlength) + "...");
	 }
	 return str;
}

console.log(truncate("hello, this is gardon valley,", 10));
console.log(truncate("hello world", 20));
```

>#### 数学函数

#### **1.写一个函数`limit2`，保留数字小数点后两位，四舍五入， 如:**

```javascript
// var num1 = 3.456
// limit2( num1 );  //3.46
// limit2( 2.42 );    //2.42

function limit2 (num) {
	 return (Math.round(num*100))/100;
}
var num1 = 3.456
console.log(limit2(num1));  //3.46
console.log(limit2(2.42));    //2.42
```


#### **2.写一个函数，获取从min到max之间的随机数，包括min不包括max**

```javascript
function myRandom (min,max) {
	 return Math.floor(Math.random()*(max-min)+min);
}
console.log(myRandom(2,8));
```


#### **3.写一个函数，获取从min都max之间的随机整数，包括min包括max**

```javascript
function myRandom (min,max) {
	 return Math.floor(Math.random()*(max-min+1)+min);
}
console.log(myRandom(2,8));
```

#### **4.写一个函数，获取一个随机数组，数组中元素为长度为len，最小值为min，最大值为max(包括)的随机数**

```javascript
function numArray (len,min,max) {
	 var arr = [];
	 for(var i=0;i<len;i++){
		arr.push(Math.floor(Math.random()*(max-min+1)+min))
	 }
	 return arr;
}
var arr1 = numArray(10,11,100);

console.log(arr1);
console.log(numArray(6,2,3));
```
---
