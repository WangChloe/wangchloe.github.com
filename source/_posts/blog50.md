---
title: visibilitychange监听回到当前webview
categories: 前端札记
tags: [webview]
date: 2019-05-24 14:47:24
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---


<!-- more -->


``` javascript
appSubsribe: function() {
    var that = this;

    document.addEventListener("visibilitychange", function(){
        if(!document.hidden) {
            that.fetchData();
        }
    });
}
```

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)