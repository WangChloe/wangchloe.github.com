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

### swiper曝光问题

- 非懒加载

``` javascript
<script>
    function bindSwiper() {
        var $swMore = $("#J_swiper_more");

        var swiperMore = new Swiper($swMore, {
            slidesPerView: "auto",
            spaceBetween: 0,
            onTouchEnd: function() {
                bindSwiperExpo($swMore);
            }
        });
    }

    function bindSwiperExpo($container){
        var Exposure = UBT.PlugIns.Exposure;
        var $images, $img;

        $images = $container.find('img').filter("[data-expo]");
        $images.each(function() {
            $img = $(this);
            Exposure.__inViewport($img) && Exposure.prepareToSend($img.data("expo"));
        });
    }
</script>
```

- 异步 + 懒加载

``` javascript
<script>
    function getIntroItem() {
        var $images, tpl;
        $.getJSON(introItemUrl, function(res) {
            if (res.status == 1) {
                tpl = $("#J_sy_intro_item_tpl").html();
                $introWrapper.html(juicer(tpl, {
                    list: res.data
                }));
                setTimeout(function(){
                    // 异步出来的先绑定lazyload事件
                    $introWrapper.find(".J_lazyimg").lazyload();
                    bindSwiper();
                    UBT.PlugIns.Exposure.init();
                },50);
            } else {
                Toast.open(res.info);
            }
        });
    }

    function bindSwiper() {
        var $swiperIntro = $("#J_swiper_intro");
        var swiper = new Swiper($swiperIntro, {
            slidesPerView: "auto",
            spaceBetween: 0,
            onTouchEnd: function() {
                bindSwiperExpo($swiperIntro);
            }
        });
    }

    function bindSwiperExpo($container){
        var Exposure = UBT.PlugIns.Exposure;
        var $images, $img;

        $images = $container.find('img');
        // 强制视线中的J_lazyimg出现
        $images.filter(".J_lazyimg:in-viewport").trigger("appear");
        $images.each(function() {
            $img = $(this);
            Exposure.__inViewport($img) && Exposure.prepareToSend($img.data("expo"));
        });
    }
</script>
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)