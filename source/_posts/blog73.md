---
title: 安卓弹窗下滑带动webview下拉刷新解决方案
categories: 前端札记
tags: ['项目总结', 'js']
date: 2018-05-02 17:33:17
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

🌝抖个小机灵

<!-- more -->


``` javascript
_andoridSetWinScroll: function() {
    if (!/ip(hone|od|ad)/i.test(navigator.userAgent.toLowerCase()) && window.screenTop === 0) {
        var bodyEl = document.body;
        var winH = window.innerHeight;

        if (bodyEl.offsetHeight < winH) {
            bodyEl.style.height = winH + 1 + 'px';
        }

        window.scrollTo(0, 1);
    }
}
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)