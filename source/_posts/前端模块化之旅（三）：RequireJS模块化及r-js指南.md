---
title: 前端模块化之旅（三）：RequireJS模块化及r.js指南
date: 2016-05-19 02:26:05
categories:
  - 编程学习
  - 前端进阶
tags:
  - JavaScript
  - 模块化
---
{% fullimage http://7xrvo9.com1.z0.glb.clouddn.com/20160519/%E5%89%8D%E7%AB%AF%E6%A8%A1%E5%9D%97%E5%8C%96%E4%B9%8B%E6%97%85%E4%B8%89.jpg %}

<!--more-->
在`html`中指定了 `RrequireJS` 第一次需要加载的模块，也就是通过 `data-main` 所指定，因此一般作为主模块：如下例中 `src/js/app/` 目录下的 `index.js` 文件

```html
<!-- index.html -->
<script data-main="src/js/app/index" src="src/js/lib/require.js"></script>
```

在主模块中我们使用 `require.config()` 方法对模块的加载行为进行一些配置：

```javascript
requirejs.config({
	baseUrl: 'src/js',
	paths: {
		jquery: 'lib/jquery',
		backTop: 'com/backTop',
		jumpTo: 'com/jumpTo',
		carousel: 'com/carousel',
		exposure: 'com/exposure',
		navfloor: 'com/navfloor',
		waterfall: 'com/waterfall'
	}
});
```

- 其中 `baseUrl` 是改变基准路径，所有的模块相对于 `baseUrl` 来加载；`baseUrl` 路径以 `index.html` 所在的目录为基准；
- 其中 `requirejs.config()` 函数的 `paths` 属性指定各个模块的加载路径(相对于 `baseUrl`)，如果没有指定 `baseUrl` ，路径默认和主模块在同一目录;

采用了 RequireJS 模块化写法解决了命名冲突和依赖管理的问题，同时也增加了文件数量，这也意味着请求的增多，无疑会带来性能问题；
这时候可以使用 RequireJS 提供的打包（优化）工具 `r.js`,它可以实现前端文件的压缩与合并,减少服务器请求，进行性能优化。
具体详尽的使用方法可以参考 require.js [官方文档](http://www.requirejs.cn/docs/optimization.html#requirements),这里主要介绍下关键步骤，关于 `build.js` 配置文件的写法,如下是一个 `build.js` 文件的内容:

```javascript
({
    baseUrl: "./src/js",
    paths: {
        'jquery': 'lib/bower_components/jquery/dist/jquery.min'
    },
    name: "main",
    out: "dist/js/merge.js"
})
```

- 其中 `baseUrl` 路径设置应与主模块中 `require.config()` 方法里的 `baseUrl` 实际路径一致
- `nam` 指定的是解析入口，这里写了主模块，路径相对于前面指定的 `baseUrl`；

>下面是 requireJS 模块化和加了 `r.js` 打包的小实践：

- [效果预览](http://febox.applinzi.com/requireJS/)
- [查看代码](https://github.com/gardonlee/Some-Demo...-/tree/master/requireJS)










---
