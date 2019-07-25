---
title: 挖坑埋坑填坑
categories: 前端札记
tags: ['填坑记', 'js']
date: 2019-04-20 14:44:42
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

<!-- MarkdownTOC -->

- css3动画闪烁解决方案
- 减少DOM数量，尽可能少的使用position:relative，减少对DOM的操作
- 红米手机-圆角外背景色溢出
- 小米1s手机-android2.3以下fixed+transform无效
- 魅族手机-gmu1.0, 2.0 dialog widget导致样式破坏
- 软键盘调出后隐藏，一些fixed元素布局呈现到奇怪的位置
- box-flex不均分问题
- css取消移动端input样式

<!-- /MarkdownTOC -->

<!-- more -->


### css3动画闪烁解决方案
 强制把需要进行动画行为的dom对象在GPU中创建Layout缓存
``` css
-webkit-transform:translateZ(0);
-webkit-transform-style:preserve-3d;
-webkit-backface-visibility:hidden;
```

### 减少DOM数量，尽可能少的使用position:relative，减少对DOM的操作
 
### 红米手机-圆角外背景色溢出
对元素设置圆角及背景色 border-radius:50px;background-color:#000; 四个圆角外会溢出背景色
解决方案：增加样式background-clip:padding-box;
参考
### 小米1s手机-android2.3以下fixed+transform无效
对fixed元素使用transform,沿着y轴-100%隐藏，等待触发时机
解决方案：先将元素隐藏，在触发之前动画之前显示，有动画效果的可以见到，无动画效果的直接显示。
### 魅族手机-gmu1.0, 2.0 dialog widget导致样式破坏
dialog widget容器外宽度无效
解决方案：css改变容器min-width
### 软键盘调出后隐藏，一些fixed元素布局呈现到奇怪的位置
调出软键盘后，点击取消，页面隐藏的fixed元素浮出
解决方案：调用scrollTo重绘页面
``` javascript
setTimeout(function() {
    window.scrollTo(document.body.scrollLeft, document.body.scrollTop);
}, 0);
```
### box-flex不均分问题
[box-flex不均分问题](http://www.cnblogs.com/leolovexx/p/5857441.html)

在`fui-span0` `fui-span1`...上增加样式`width:1%;`

### css取消移动端input样式

- IOS端：

``` html
<style>
background-color:transparent; border-color:transparent;
</style>
```

- andorid端：

仅仅使用上面的代码还不够，可以发现select框在某些浏览器（包括微信）内置浏览器下 还会有箭头以及高亮的线框，要除去这个可以使用

``` html
<style>
<!--取消webkit默认的样式。-->
-webkit-appearance: none;
</style>
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)