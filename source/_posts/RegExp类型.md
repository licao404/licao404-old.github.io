---
title: RegExp类型
categories:
  - 编程学习
  - 前端基础
tags:
  - JavaScript
date: 2016-03-31 10:02:14
---
<blockquote class="blockquote-center">ECMAScript通过RegExp来支持正则表达式，正则表达式是一种描述文本内容（字符串结构）的模式，有点像字符串的模板。常常用作按照“给定模式”匹配文本的工具</blockquote>

<!--more-->
>### 【知识点】

#### **1. `\d`，`\w`,`\s`,`[a-zA-Z0-9]`,`\b`,`.`,`*`,`+`,`?`,`x{3}`,`^$`分别是什么？**

- `\d`：匹配0-9间的任意一个数字，等同于`[0-9]`；
- `\w`：匹配任意的字母、数字、下划线，等同于`[A-Za-z0-9_]`；
- `\s`：匹配空白字符（包括空格、tab、换行、回车），等同于`[\n\t\r\v\f]`；
- `[a-zA-Z0-9]`：匹配任意的字母、数字
- `\b`：匹配单词边界；
- `.`：匹配除换行、回车、结束符（行分隔符\u2028、段分隔符\u2029）以外的单个字符；
- `*`：匹配出现0次或多次，等同于`{0,}`；
- `+`：匹配出现1次或多次，等同于`{1,}`；
- `?`：匹配出现0次或1次，等同于`{0,1}`；
- `x{3}`：匹配恰好重复3次；`{n,}`表示至少重复n次；`{n,m}`表示重复不少于n次，不多于m次
- `^$`：`^`表示字符串开始的位置，`$`表示字符串结束的位置，`/^n$/`表示从开始到结束位置只有`n`
- `|`：或关系；


#### **2. 贪婪模式和非贪婪模式指什么？**

- 贪婪模式：趋向于最大长度的匹配，比如`var str = "abbbc";`,贪婪模式下，`str.match(/ab+/)`匹配到的是`abbb`；
- 非贪婪模式：匹配到结果就好，尽可能少的匹配。默认是贪婪模式，如果需要改为非贪婪模式，则在量词符（`+`,`*`,`?`）后加一个问号，`str.match(/ab+?/)`匹配到的则是`ab`；

---
>### 【练习】

#### **1.写一个函数`trim(str)`，去除字符串两边的空白字符**

```javascript
function trim (str) {
     return str.replace(/^\s+|\s+$/g,"");
}
```

#### **2.使用实现 `addClass(el, cls)`、`hasClass(el, cls)`、`removeClass(el,cls)`，使用正则**

```javascript
//提示: el为dom元素，cls为操作的class， el.className获取el元素的class

function hasClass (el,cls) {
    var exp = new RegExp("\\d"+cls+"\\d");
    return exp.test(el.className);
}

//如果不存在则进行添加
function addClass (el,cls) {
     if (!hasClass(el,cls)) {
        return el.className += " " + cls;
      }
}

//如果存在则进行删除
function removeClass (el,cls) {
     if (hasClass(el,cls)) {
        return el.className.replace(new Regexp("\\d"+cls+"\\d"), '');
     }
}
```

#### **3.写一个函数`isEmail(str)`，判断用户输入的是不是邮箱**

```javascript
function isEmail (str) {
    //为了方便Console测试效果，下面所有函数均不return，而是用if(){}else{}输出查看
     if(str.match(/^([\w\.\-])+\@(([\w\-])+\.)+([a-zA-Z0-9]{2,4})$/)){
        console.log("email格式 正确");
     }else {
        console.log("email格式 错误");
     }
}
```

#### **4.写一个函数`isPhoneNum(str)`，判断用户输入的是不是手机号**

```javascript
function isPhoeNum (str) {
    if (str.match(/^1[34578]\d{9}$/)) {
       console.log("手机号格式 正确");
     } else {
       console.log("手机号格式 错误");
     }
}
```

#### **5.写一个函数`isValidUsername(str)`，判断用户输入的是不是合法的用户名（长度6-20个字符，只能包括字母、数字、下划线）**

```javascript
function isValidUsername (str) {
    if (str.match(/^\w{6,20}$/)) {
       console.log("用户名 正确");
    }else {
       console.log("用户名 错误");
    }
}
```

#### **6.写一个函数`isValidPassword(str)`, 判断用户输入的是不是合法密码（长度6-20个字符，包括大写字母、小写字母、数字、下划线至少两种）**

```javascript
function isValidPassword (str) {

       // ?!exp 零宽度正先行断言，排除全数字，全大写字母，全小写字母，全下划线
        if (str.match(/^(?!^\d+$)(?!^[A-Z]+$)(?!^[a-z]+$)(?!^[_]+$).{6,20}$/) && str.match(/^\w{6,20}$/)) {
           console.log("密码格式 正确");
        }else {
           console.log("密码格式 错误");
        }
}
```

#### **7.写一个正则表达式，得到如下字符串里所有的颜色`（#121212）`**

```javascript
var re = /#[\da-f]{6}/ig;

var subj = "color: #121212; background-color: #AA00ef; width: 12px; bad-colors: f#fddee #fd2 "

alert( subj.match(re) )  // #121212,#AA00ef
```

#### **8.下面代码输出什么? 为什么? 改写代码，让其输出`gardon`,`world`.**

```javascript
var str = 'hello  "gardon" , hello "world"';
var pat =  /".*"/g;
str.match(pat);  
//输出 "gardon" , hello "world"
//量词符*默认贪婪模式，/".*"/g匹配第一个 " 到最后一个 " 之间除换行、回车、结束符（行分隔符\u2028、段分隔符\u2029）以外的所有字符
```

```javascript
//量词符后加?改写为非贪婪模式
var str = 'hello  "gardon" , hello "world"';
var pat =  /".*?"/g;
str.match(pat);
//输出 "gardon","world"
```

#### **9.补全如下正则表达式，输出字符串中的注释内容. (可尝试使用贪婪模式和非贪婪模式两种方法)**

```javascript
str = '.. <!-- My -- comment \n test --> ..  <!----> .. ';

//贪婪模式
re = /<!--[^>]*-->/g;

//非贪婪模式
re = /<!--(.|\s)*?-->/g;
// re = /<!--[^>]*?-->/g;

str.match(re) ;// '<!-- My -- comment \n test -->', '<!---->'
```

#### **10.补全如下正则表达式**

```javascript
//贪婪模式
var re =/<[^>]+>/g;

//非贪婪模式
var re = /<[^>].*?>/g;

var str = '<> <a href="/"> <input type="radio" checked> <b>'
str.match(re) // '<a href="/">', '<input type="radio" checked>', '<b>'
```
---
