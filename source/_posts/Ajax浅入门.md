---
title: AJAX-从入门到放弃
categories:
  - 编程学习
  - 前端基础
tags:
  - JavaScript
  - AJAX
  - 前后端
date: 2016-04-12 00:47:32
---
>### 【知识点】

#### **AJAX 是什么？有什么作用？**
AJAX（**Asynchronous Javascript And XML**）即异步的JavaScript和XML，是一种创建交互式网页应用的网页开发技术，所以并不是一门编程语言（小白多少年以为它诸如JavaScript，jQuery一类`(@_@;)`,身份终于得到正名）。

通过在后台与服务器进行少量的数据交换，AJAX可以使网页实现异步更新，这意味着可以在不重新加载（刷新）整个网页的情况下，对网页某部分进行更新。
<!--more-->
AJAX的核心、基础是JavaScript的 **XMLHttpRequest** 对象（`IE7+、Firefox、Chrome、Safari and Opera`），这个对象为向服务器发送请求和解析服务器响应提供了流畅的接口，XmlHttpRequest可以使用JavaScript向服务器提出请求并处理响应，而不阻塞用户。通过以下代码创建一个XMLHttpRequest对象：
```javascript
var xmlhttp = new XMLHttpRequest();
```

老版本IE浏览器（IE5，6）使用`ActiveX`对象：
```javascript
var xmlhttp = new ActiveXObject();
```

尝试写一个浏览器兼容的创建XHR对象的函数：
```javascript
function creatXHR(){
    var xmlhttp;
    if (window.XMLHttpRequest) {
        //IE7+、Firefox、Chrome、Safari and Opera
        xmlhttp = new XMLHttpRequest();
    }else{
        //IE5，6
        xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
    }
    return xmlhttp;
}
```
>这个函数貌似只是个思路(草稿)？我们试试让他更加丰满的吧，教你封装一个“<a href="#01">ajax()函数</a>”.


创建了XMLHttpRequest对象后，我们需要给对象的 **onreadystatechange** 属性绑定事件监听函数，告诉服务器接收到请求然后应该去干什么：
```javascript
xmlhttp.onreadystatechange = function(){
    //Something you want to do ...
    console.log(xmlhttp.status);
    if (xmlhttp.readyState == 4 && xmlhttp.status == 200) {
        //do something
    }
    if(xmlhtt.status === 404){
        //do something
    }
};
```

然后发送请求，有两个重要方法 **open()** 和 **send()** ：
```javascript
xmlhttp.open("GET","example.php",true)
```

>`open()`方法内的3个参数：
**`1).`** `method`规定HTTP请求的方式，一般是`GET`或者`POST`（按照HTTP规范,该参数要大写;否则,某些浏览器(如Firefox)可能无法处理请求。HTTP有[9种请求方式](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)
**`2).`** 请求页面的`url`，注意不能**跨域请求**,否则会有`permission denied`错误；
**`3).`** `asunc`:设置为 **true** 异步模式，JavaScript函数将继续执行,而不等待服务器响应.这就是"AJAX"中的"A"；设置为 **false** 同步模式，只能等待服务器响应而无法继续进行其他；

```javascript
xmlhttp.send();
xmlhttp.send(string);
```

>如果采用了`GET`方式，直接`send()`后进行其他操作;如果采用了`POST`方式，`send()`方法的参数可以使任何想发给服务器的数据，规定必须是字符串的形式；

**readyState** ：
- 0 ：请求未初始化
- 1 ：服务器连接已建立
- 2 ：请求已接收
- 3 ：请求处理中
- 4 ：请求完成，响应就绪

**status** ：
- 202 ： “OK”
- 404 ： 未找到资源




#### **前后端开发联调需要注意哪些事情？后端接口完成前如何 mock 数据？**
前后端纯接口开发模式：
**`1).`** 在开发之前规定好接口文档，并指明由谁来撰写和维护；
**`2).`** 如果接口的信息改动需要互相确认改动信息；
**`3).`** 确定接口数据传输类型，JSON、XML or JSONP；
**`4).`** 定义数据管理和归属权，是属于前端管理还是后端管理；
**`5).`** 确定数据传输方式，前后端之间是否有中间层；

后端接口完成前如何 mock 数据：
**`1).`** 自己制作模拟数据，但这种方式工作量比较大，而且需要收集分类要求的数据，如果`API`变更，之前的所有数据也需要及时更新。但这样的好处是能够快速的完成前端任务；
**`2).`** 使用模拟数据生成器：[Mock.js](http://mockjs.com/)；


#### **点击按钮，使用 AJAX 获取数据，如何在数据到来之前防止重复点击?**
**`1).`** 这个只知道可以通过设置一个表示状态的`flag`，初始化值为`false`，即：
```javascript
var flag = false;
```
**`2).`** 然后在绑定事件函数内部flag值设为`true`，当前可用,函数返回。例如：
```javascript
btn.addEventListener('click', function(e) {
    //......
    flag = true;
    if(flag){
        return;
    }
});
```
**`3).`** 请求数据时，函数flag设为false，保证其重复点击不请求。
......待续


---
>### 【练习】

#### **封装一个 AJAX 函数，能通过如下方式调用**
```javascript
document.querySelector('#btn').addEventListener('click', function(){
    ajax({
        url: 'getData.php',   //接口地址
        type: 'get',               // 类型， post 或者 get,
        data: {
            username: 'xiaoming',
            password: 'abcd1234'
        },
        success: function(ret){
            console.log(ret);       // {status: 0}
        },
        error: function(){
           console.log('出错了')
        }
    })
});
```
<a id="01" name="01"> </a>

>将其封装成一个[ajax.js](https://github.com/jirengu-inc/jrg-tehui2/blob/master/homework/%E6%9D%8E%E6%93%8D/task24/js/ajax.js)，以便让我们以后尽情的调用它，代码如下：

```javascript
var xmlhttp = false;

function ajax(opts) {
    //跨浏览器创建xhr对象
    xmlhttp = false;
    if (window.XMLHttpRequest) { //现代浏览器
        xmlhttp = new XMLHttpRequest();
        if (xmlhttp.overrideMimeType) { //设置MIME类别，针对某些特定版本的mozillar浏览器的BUG进行修正
            xmlhttp.overrideMimeType('text/xml');
        }
    } else if (window.ActiveXObject) { //IE5,6
        try {
            xmlhttp = new ActiveXObject("Msxm12.XMLHTTP");
        } catch (e) {
            try {
                xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
            } catch (e) {}
        }
    }

    if (!xmlhttp) { //异常，创建xhr对象实例失败
        alert('Giving up :( Cannot create an XMLHTTP instance');
        return false;
    }


    xmlhttp.onreadystatechange = function() {
        if (xmlhttp.readyState == 4 && xmlhttp.status == 200) {
            var json = JSON.parse(xmlhttp.responseText);
            opts.success(json);
        }
        if (xmlhttp.readyState == 4 && xmlhttp.status == 404) {
            opts.error();
        }
    }

    var dataStr = '';
    for (var key in opts.data) {
        dataStr += key + '=' + opts.data[key] + '&';
    }
    // dataStr = dataStr.substr(0,dataStr.length-1);
    dataStr = dataStr.replace(/&$/g, '');

    if (opts.type.toLowerCase() === 'post') {
        xmlhttp.open(opts.type, opts.url, true);
        xmlhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
        xmlhttp.send(dataStr);
    }
    if (opts.type.toLowerCase() === 'get') {
        xmlhttp.open(opts.type, opts.url + '?' + dataStr, true);
        xmlhttp.send();
    }

}
```


#### **实现如下"加载更多"的功能**
- [效果点我点我](http://febox.applinzi.com/task24-2/task24-2.html)
- 瞅瞅代码：[index.html](https://github.com/licao404/landemo/blob/master/task24-2/task24-2.html) | [ajax.js](https://github.com/licao404/landemo/blob/master/js/ajax.js)

#### **实现注册表单验证功能**
- [效果点我点我](http://febox.applinzi.com/task24-3/task24-3.html)
- 瞅瞅代码：[index.html](https://github.com/licao404/landemo/blob/master/task24-3/task24-3.html) | [ajax.js](https://github.com/licao404/landemo/blob/master/js/ajax.js) | [checkIpt.js](https://github.com/licao404/landemo/blob/master/js/checkIpt.js) | [doClass.js](https://github.com/licao404/landemo/blob/master/js/doClass.js)

---
>参考:

- [MDN | AJAX入门](https://developer.mozilla.org/zh-CN/docs/AJAX/Getting_Started)

---
