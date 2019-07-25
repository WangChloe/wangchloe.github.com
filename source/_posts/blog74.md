---
title: 初识proxy
categories: 前端札记
tags: js
date: 2018-05-07 17:36:15
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

👑this问题解决扛把子

<!-- MarkdownTOC -->

- this对象更改
	- 优化：$.proxy函数该改变上下文\(context\)语境
- 设置变量指向this
	- 优化：bind\(this\)
	- 优化：$.proxy

<!-- /MarkdownTOC -->


<!-- more -->

[$.proxy使用](https://www.cnblogs.com/hongchenok/p/3919497.html)

### this对象更改

``` javascript
var that = this;

this.$mask.on("touchmove", function (ev) {
    ev.preventDefault();
}).on("click", function () {
    that.deactivate();
});
```

#### 优化：$.proxy函数该改变上下文(context)语境

``` javascript
this.$mask.on("touchmove", function (ev) {
    ev.preventDefault();
}).on("click", this.proxy(function () {
    this.deactivate();
}));
```

### 设置变量指向this

``` javascript
$('#myElement').click(function() {
    var that = this;   //设置一个变量，指向这个需要的this

    setTimeout(function() {

          // 这个this指向的是settimeout函数内部，而非之前的html元素

        $(that).addClass('aNewClass');

    }, 1000);

});
```

#### 优化：bind(this)

``` javascript
$('#myElement').click(function() {

    setTimeout(function() {

        $(this).addClass('aNewClass');

    }.bind(this), 1000);

});
```

#### 优化：$.proxy

``` javascript
$('#myElement').click(function() {

    setTimeout($.proxy(function() {

        $(this).addClass('aNewClass');

    }, this), 1000);

});
```

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)