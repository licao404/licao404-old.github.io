---
title: Date类型/引用类型深拷贝
date: 2016-03-26 20:05:19
categories: [编程学习,前端基础]
tags: JavaScript
---
>### 【知识点】

#### **1. 基础类型有哪些？复杂类型有哪些？有什么特征？**

- 基础类型：数值、字符串、布尔值、undefined、null；
- 复杂类型：函数、对象、数组、正则、Date……  

<!--more-->  
>- 基本类型的数据是直接保存在栈内存里的，按值访问，操作实实在在保存的值；
>- 引用类型保存在堆内存内,变量保存的是它们所在堆内存中的地址（指针）；
>- 基本类型间的复制是值的复制，一个值发生改变不会影响另一个，如：
```javascript
var a = 1;
var b = a;//a复制给b
a = 2;//改变a
console.log("a = "+a);//a = 2
console.log("b = "+b);//b = 1 a发生改变 b不会变
```
- 正是因为变量保存的是指针，所以变量间复制的是栈内存里的指针，是指向堆中同一个对象，所以其中一个改变必然另外一个也要改变,如下图所示：
![](http://7xr868.com1.z0.glb.clouddn.com/task19%2F%E5%BC%95%E7%94%A8%E7%B1%BB%E5%9E%8B%E5%A4%8D%E5%88%B6.jpg)

---
>### 【练习】

#### **1.写一个函数getIntv，获取从当前时间到指定日期的间隔时间**

```javascript
function getIntv (date) {
     var allms = Date.parse(date) - Date.now(); //当前时间距离目标时间的总毫秒数
     var dayms = 1000*60*60*24,//1天
         houms = 1000*60*60,//1小时
         minms = 1000*60,//1分钟
         secms = 1000;//1秒
     var day = Math.floor(allms/dayms),
         hou = Math.floor((allms%dayms)/houms),
         min = Math.floor(((allms%dayms)%houms)/minms),
         sec = Math.floor((((allms%dayms)%houms)%minms)/secms);
     return "距离除夕还有 "+day+" 天 "+hou+" 小时 "+min+" 分 "+sec+" 秒";
}

var str = getIntv("2017-01-27");//2017年除夕
console.log(str);// 距除夕还有 ？ 天 ？ 小时 ？ 分 ？ 秒
```

#### **2.把数字日期改成中文日期**

```javascript
function getChsDate (date) {
     var dateObj = new Date(date);
     var list = ['零', '一','二','三','四','五','六','七','八','九','十','十一','十二','十三','十四','十五','十六','十七','十八','十九','二十','二十一','二十二','二十三','二十四','二十五','二十六','二十七','二十八','二十九','三十','三十一'];
     var year = dateObj.getFullYear().toString(),
         mon = dateObj.getMonth()+1,
         day = dateObj.getDate();
     return list[year[0]] + list[year[1]] + list[year[2]] + list[year[3]] + ' 年 ' + list[mon] +' 月 ' + list[day] + ' 日 ';
}
var str = getChsDate('2015/2/19');
console.log(str);// 二零一五年二月十九日
```

#### **3.写一个函数获取n天前的日期**

```javascript
function getLastNDays (num) {
      var datems = Date.now() - num*(24 * 60 * 60 * 1000),
          date = new Date(datems),
          year = date.getFullYear(),
          mon = date.getMonth()+1,
          day = date.getDate();
      return year + '-' + mon + '-' + day;
}
var lastWeek =  getLastNDays(7);
console.log(lastWeek);
var lastMonth = getLastNDays(30);
console.log(lastMonth);
```

#### **4.完善如下代码，如：**

```javascript
var Runtime = (function(){
    var startTime,endTime,doTime;
    return {
        start: function(){
              startTime = Date.now();
              return startTime;
        },
        end: function(){
              endTime = Date.now();
              return endTime;
        },
        get: function(){
             doTime = endTime - startTime;
             return '运行时间:'+ doTime + 'ms';
        }
    };
}());
Runtime.start();
for(var i = 0; i<10000; i++){
    console.log(1);
}
Runtime.end();
console.log(  Runtime.get() );
```

#### **5.楼梯有200级，每次走1级或是2级，从底走到顶一共有多少种走法？用代码（递归）实现**

```javascript
//这应该是一个经典的递归问题，不过200级对于递归来说效率问题就……，试了下算50级在我这渣电脑上运行了6分多钟以上
Runtime.start();
function walkWay(step) {
    return (step == 0 || step == 1)?1:walkWay(step - 1) + walkWay(step - 2);
}
console.log('总共 ' + walkWay(50) + ' 走法');
Runtime.end();
console.log(  Runtime.get() );
```
#### **6.写一个json对象深拷贝的方法，json对象可以多层嵌套，值可以是字符串、数字、布尔、json对象中的任意项**

```javascript
function jsonCopy (json) {
     var newJson = {};
     for (var k in json) {
        if(typeof json[k] == "object"){
            newJson[k] = jsonCopy(json[k]);
        }else {
            newJson[k] = json[k];
        }
     }
     return newJson;
}
var studentA = {
    "name":"licao",
    "age":"20",
    "like":{
        "food":"meat",
        "drinking":"milk"
    }
};
var studentB = jsonCopy(studentA);
console.log(studentA);//Object {name: "licao", age: "20", like: Object}
console.log(studentB);//Object {name: "licao", age: "20", like: Object}
```

#### **7.写一个数组深拷贝的方法，数组里的值可以是字符串、数字、布尔、数组中的任意项目**

```javascript
function arrCopy (arr) {
     var newArr = [];
     for (var k in arr) {
        if (arr[k] instanceof Array) {
            newArr[k] = arrCopy(arr[k]);
        }else {
            newArr[k] = arr[k];
        }
      }
      return newArr;
}
var studentA = ['licao','20','true',['licao','20','true']];

console.log(studentA);//["licao", "20", "true", Array[3]]
console.log(arrCopy(studentA));//["licao", "20", "true", Array[3]]
```

#### **8.写一个深拷贝的方法，拷贝对象以及内部嵌套的值可以是字符串、数字、布尔、数组、json对象中的任意项**

```javascript
function objectCopy (obj) {
     var newObj = {};
     for (var k in obj) {
        if (typeof obj[k] == "object") {
            newObj[k] = objectCopy(obj[k]);
        }else {
            newObj[k] = obj[k];
        }
      }
      return newObj;
}
var studentA = {
    name:"licao",
    age:20,
    man:true,
    other:{
        "hobby":"shoot",
        "birth":1995
    },
    arr:[1,2,3]
};
console.log(studentA);//Object {name: "licao", age: 20, man: true, other: Object, arr: Array[3]}
console.log(objectCopy(studentA));//Object {name: "licao", age: 20, man: true, other: Object, arr: Array[3]}

```

---
