---
title: swiper小记
date: 2017-07-08 03:27:35
categories: 前端札记
tags: [js]

---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

🦁移动端轮播神器～

<!-- more -->


### swiper隐藏后不轮播问题

- observer
- observerParents

```
swiper = new Swiper('.swiper-container', {
    autoplay: 3000, //可选选项，自动滑动
    pagination: '.swiper-pagination',
    autoplayDisableOnInteraction: false,
    observeParents: true,
    observer: true.
});
```

上述方法无效，还是得重new一个swiper

找到下述方法，stop/start autoplay

```
swiper.stopAutoplay();
```

```
swiper.startAutoplay();
```

### 安卓swiper禁止带动下拉刷新

`followFinger: Fanli.Utility.isIos`

```
swiper = new Swiper('.swiper-container', {
    autoplay: 3000, //可选选项，自动滑动
    pagination: '.swiper-pagination',
    autoplayDisableOnInteraction: false,
    followFinger: Fanli.Utility.isIos
});
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)