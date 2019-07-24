---
title: 更改微信内分享链接
date: 2019-05-10 03:27:35
categories: 前端札记
tags: [微信]

---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

<!-- more -->

``` javascript
wx.ready(function () {
    // 分享给朋友
    wx.onMenuShareAppMessage({
        link: shareUrl,
        title: wxFanliConfig.session.title,
        desc: wxFanliConfig.session.desc,
        imgUrl: wxFanliConfig.session.img,
        success: function (res) {
            if (wagv.inWeiXin && typeof wagv.inWeiXin["shareAppSuccess"] == "function") {
                wagv.inWeiXin["shareAppSuccess"]();
            }
        },
        fail: function (res) {
            alert('分享失败');
        }
    });

    // 分享到朋友圈
    wx.onMenuShareTimeline({
        link: shareUrl,
        title: wxFanliConfig.timeline.title,
        desc: wxFanliConfig.timeline.desc,
        imgUrl: wxFanliConfig.timeline.img,
        success: function (res) {
            if (wagv.inWeiXin && typeof wagv.inWeiXin["shareTimelineSuccess"] == "function") {
                wagv.inWeiXin["shareTimelineSuccess"]();
            }
        },
        fail: function (res) {
            alert('分享失败');
        }
    });

});
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)