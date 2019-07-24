---
title: 软键盘搜索及事件
date: 2017-07-01 03:27:35
categories: 前端札记
tags: [js]

---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---


<!-- more -->

```
<form action="" onsubmit="return false;">
    <input type="search" value="" placeholder="搜索你想要的" id="search">
</form>
```

```
$('#search').keydown(function(event){
    if(event.keyCode==13){
    var keyword  = $('#search').val();
        alert(keyword);
    }
});
```



---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)