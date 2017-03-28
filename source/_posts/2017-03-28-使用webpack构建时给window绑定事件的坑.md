---
title: 使用webpack构建时给window绑定事件的坑
date: 2017-03-28 15:45:25
tags: 
- webpack
- jquery
categories: 填坑
---

使用webpack进行构建，给window绑定了onload的事件，结果竟然死活不触发！各种排除后发现问题所在...

<!-- more -->

业务js文件`index.js`

```javascript
var $ = require('jquery');

$(function(){
    // 在这里绑定，无效
    $(window).on('load', function(){
        console.log('页面加载完成了~');
    });
})
```

打开页面，然后控制台并没有打印出“页面加载完成了~”

难道是使用jquery进行绑定的原因导致事件绑定无效？于是改了代码：

```javascript
var $ = require('jquery');

$(function(){
    // 在这里绑定，还是无效
    window.onload = function(){
        console.log('页面加载完成了~');
    };
})
```

控制台依然没有打印出预期的结果，苦思良久，改成

```javascript
var $ = require('jquery');

// 在这里绑定，有效
$(window).on('load', function(){
    console.log('页面加载完成了~ 1');
});

// 在这里绑定，也有效
window.onload = function(){
    console.log('页面加载完成了~ 2');
};

$(function(){
    
})
```

那看来是'$(function(){})'这一段代码的问题了，这段代码并不难理解，当页面的dom结构加载完毕则执行，刚入门是使用'script'标签引入jquery，并没有这样的问题出现。

那么现在看来是使用webpack进行构建的并发症了，至于原因，并没有深究的意愿。

现在页面的js基本都是放在页面底部，程序执行到这里，其实意味着dom结构以及渲染完毕了，个人觉得在js中写'$(function(){})'是没有必要了的