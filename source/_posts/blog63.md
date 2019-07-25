---
title: 拖拽衍生记
categories: 前端札记
tags: ['特效', 'js']
date: 2018-09-30 14:39:15

---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

<!-- MarkdownTOC -->

- 满屏拖拽
- 纵向贴边拖拽

<!-- /MarkdownTOC -->

<!-- more -->

### 满屏拖拽
``` javascript
(function() {
    var $root = $("#J_drag_trigger");
    var hasBottomNav = $(".J_bottom_nav").length;

    if (!$root.length) {
        return;
    }

    var $win = $(window);
    var elWidth = $root.width();
    var elHeight = $root.height();
    var disX = window.screen.availWidth - elWidth;
    var disY = window.screen.availHeight - elHeight;

    if(hasBottomNav) {
        disY = disY - $(".J_bottom_nav").height();
    }

    $root.on("touchstart", function(ev){
        if($win.scrollTop() == 0) {
            window.scrollTo(0, 1);
        }
    }).on("touchmove", function(ev) {
        onMove(ev);
    }).on("touchend", function(ev){
        onMove(ev, 'end');
    });

    function onMove(ev, status) {
        var e = ev.originalEvent.changedTouches[0];;
        var x = e.pageX - elWidth / 2;
        var y = e.pageY - $win.scrollTop() - elHeight / 2;

        x < 0 && (x = 0);
        y < 0 && (y = 0);

        x > disX && (x = disX);
        y > disY && (y = disY);

        status != "end" && ev.preventDefault();
        $root.css({
            "transform": "translate(0,0)",
            "left": x,
            "top": y,
            "z-index": 2000
        });
        if(status == "end"){
            x = e.pageX >= (disX + elWidth)/2 ? disX : 0 ;
            $root.animate({
                "left": x
            }, 200)
        }
    }

})();
```
### 纵向贴边拖拽

``` html
<style>
.btn-coupon{z-index:9;position:fixed;top:50%;right:-1.27rem;margin-top:-.38rem;width:1.27rem;height:.76rem;background:url(/super/src/h5/images/brand/btn-coupon.png);background-size:1.27rem .76rem;transition:.5s;}
.btn-coupon.show{transform:translateX(-1.27rem);}
</style>
```

``` html
<div class="J_btn_coupon J_coupon_dialog_trigger btn-coupon"></div>
```

``` javascript
function bindBtnCouponSlide() {
    var $btnCoupon = $(".J_btn_coupon");
    var $slideWrapper = $(".J_slide_wrapper");
    var $itemMod = $slideWrapper.find(".J_item_mod");

    var wrapperT = $slideWrapper.offset().top;
    var wrapperH = $slideWrapper.height();
    var modH = $itemMod.height();

    var elHeight = $btnCoupon.height();
    var disY = window.screen.availHeight - elHeight;
    var scrollTop, e, y;

    $doc.on("touchmove", function(ev) {
        scrollTop = $win.scrollTop();
        if (ev.target == $btnCoupon.get(0)) {
            ev.preventDefault();

            e = ev.changedTouches[0];
            y = e.pageY - $win.scrollTop() - elHeight / 2;

            y < 0 && (y = 0);
            y > disY && (y = disY);

            $btnCoupon.css("top", y);
        } else {
            $btnCoupon.removeClass("show");
        }
    }).on("touchend", function() {
        if (scrollTop > wrapperT && scrollTop < wrapperT + wrapperH- modH && !$couponDialog.hasClass("show")) {
            $btnCoupon.addClass("show");
        } else {
            $btnCoupon.removeClass("show");
        }
    });
}
```

- 改版 兼容ios/android touchend

``` javascript
function bindBtnCouponSlide(isStart) {
    var $slideWrapper = $(".J_slide_wrapper");
    var $itemMod = $slideWrapper.find(".J_item_mod");
    var dragClass = "drag";

    if (!$btnCoupon.length || !isStart || $couponWrap.hasClass("super-coupon-presale")) {
        return;
    }

    var wrapperT = $slideWrapper.offset().top;
    var wrapperH = $slideWrapper.height();
    var maxH = wrapperT + wrapperH;

    var elHeight = $btnCoupon.height();
    var elWidth = $btnCoupon.width();
    var disY = window.screen.availHeight - elHeight;
    var scrollTop, e, y;

    $doc.on("touchmove", function(ev) {
        e = ev.changedTouches[0];
        y = e.pageY - $win.scrollTop(); - elHeight / 2;

        y < 0 && (y = 0);
        y > disY && (y = disY);

        if (ev.target == $btnCoupon.get(0)) {
            ev.preventDefault();
            $btnCoupon.addClass(dragClass);
            $btnCoupon.css("top", y);
        } else {
            $btnCoupon.removeClass(showClass);
        }
    });

    if (Fanli.Utility.isIos) {
        $doc.on("touchstart", function(ev) {
            if (ev.target != $btnCoupon.get(0)) {
                $btnCoupon.hide().removeClass(showClass);
            }
        })

        $win.on("scroll", function() {
            scrollTop = $win.scrollTop();
            if (scrollTop > wrapperT && scrollTop < maxH) {
                $btnCoupon.show().addClass(showClass);
            } else {
                $btnCoupon.removeClass(showClass).removeClass(dragClass);
            }
        })
    } else {
        $doc.on("touchend", function() {
            scrollTop = $win.scrollTop();
            if (scrollTop > wrapperT && scrollTop < maxH) {
                $btnCoupon.show().addClass(showClass);
            } else {
                $btnCoupon.removeClass(showClass).removeClass(dragClass);
            }
        });
    }
}
```

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)