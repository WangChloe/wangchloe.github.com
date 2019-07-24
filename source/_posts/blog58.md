---
title: 数字速变特效
categories: 前端札记
tags: ['特效', 'js']
date: 2018-12-11 17:12:06
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

嚓嚓嚓 顿顿顿
<!-- more -->

效果如下

![](http://ww3.sinaimg.cn/large/006tNc79gy1g5b1ulk5ydg30hs08we6m.gif)


``` javascript
bindNumScroll: function(val, initVal) {
    var that = this;
    var current = 0;
    var step = val >= 400 ? Math.ceil(val / 100) : Math.ceil(val / 10);
    var start = parseFloat(initVal);

    timer = setInterval(function() {
        start += step;
        if (start > val) {
            clearInterval(timer);
            start = val;
            timer = null;
        }
        if (current === start) {
            return;
        }
        current = start;
        $numBounty.html(current.toString().replace(/(\d)(?=(?:\d{3}[+]?)+$)/g, '$1,'));
    }, 10);
}
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)