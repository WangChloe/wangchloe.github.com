---
title: 图片预加载方案
categories: 前端札记
tags: ['性能优化', 'js']
date: 2017-12-25 15:35:24
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

对js分布动画尤其有效


<!-- more -->

``` javascript
(function(){
    var prefix = '//domain.com';
    var preLoadImgs = ['date.alpha.png', 'box5.alpha.png', 'box4.alpha.png', 'box3.alpha.png', 'box2.alpha.png', 'box1.alpha.png', 'fore-prod.alpha.png'];
    var preLen = preLoadImgs.length;
    var holdImages = [];
    var i = 0;

    var $gifWrap = $("#J_gif_wrap");
    var str = '<img class="date animated tada infinite" src="../date.alpha.png">' +
    '<img class="box box5 animated slideInDown" src="../box5.alpha.png" />' +
    '<img class="box box4 animated slideInDown" src="../box4.alpha.png" />' +
    '<img class="box box3 animated slideInDown" src="../box3.alpha.png" />' +

    '<img class="box box2 animated slideInDown" src="../box2.alpha.png" />' +
    '<img class="box box1 animated slideInDown" src="../box1.alpha.png" />' +

    '<img class="fore-prod animated slideInDown" src="../fore-prod.alpha.png" />';

    $.each(preLoadImgs, function(idx, item) {
        var img = new Image();

        img.onload = img.onerror = function() {
            i++;
            img.onload = img.onerror = null;
            if(i >= preLen) {
                $gifWrap.append(str);
            }
        }

        holdImages.push(img);
        img.src = prefix + item;
    });
})();
```

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)