---
title: 承接页问题汇总
categories: 前端札记
tags: 项目总结
date: 2019-03-25 15:00:04
updated: 2019-07-24 15:00:04
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---


<!-- more -->

## 页面结构

- 承接页
  - 单个商品+品牌一排一个商品+猜你喜欢一排两列商品
  - ![](https://ws1.sinaimg.cn/large/006tKfTcgy1g1etu8gs40j30u03q6b2c.jpg)
  - 置顶效果
  - ![](https://ws1.sinaimg.cn/large/006tKfTcgy1g1etv6irx8j30u01hd1kx.jpg)

- 品牌页
  - 品牌一排一个商品+猜你喜欢一排两列商品
  - ![](https://ws2.sinaimg.cn/large/006tKfTcgy1g1etvs203qj30u02uzx6q.jpg)
  - 置顶效果
  - ![](https://ws2.sinaimg.cn/large/006tKfTcgy1g1etvuyy5lj30u02uzhdv.jpg)


## 遇到问题

### 1. 有fixed bar的情况下还需优惠券置顶

- 方案1: append `$(".page-bar").append($("#J_coupon"))`✘
  - 优点：不需要为`$("#J_coupon")`增加fixed样式
  - 缺点：如果券已向右滑动，不便于保持滑动状态，需iscroll/记录translateX(..)

- 方案2: `$("#J_coupon.fixed")` ✔︎
  - 优点：能保持券状态
  - 缺点：需要为`$("#J_coupon")`增加fixed样式

### 2. 滚动需监听事件颇多 (目前方案不是最优)
- **bar**
- **券**
- **查看更多xx件商品**
- **右下角置顶按钮**
  - 品牌商品数量 > 20
    - 下滑 展示当前已浏览数量
    - 上滑 展示置顶按钮
  - 品牌商品数量 <= 20
    - 展示置顶按钮

``` javascript
bindScroll: function() {
    var $relate = $("#J_relate");
    var $brand = $("#J_brand");
    var $coupon = $("#J_coupon");
    var $couponSpacer = $("#J_coupon_spacer");
    var barHeight = $("#J_page_bar").outerHeight();
    brandTop = $brand.length && $brand.offset().top > 0 ? ($brand.offset().top - (this.isPageBrand ? barHeight : 0)) : 0;
    couponTop = $couponSpacer.length ? $couponSpacer.offset().top - barHeight : 0;
    var relateTop = $relate.length && $relate.offset().top > 0 ? this.isBrandManji ? $relate.offset().top - (this.isPageBrand ? (barHeight + $coupon.outerHeight() ) : (barHeight + $coupon.height() - parseFloat(.3 * htmlFS))) : $relate.offset().top - barHeight : 0;
    var brandColH = this.isBrandManji ? parseFloat(2.6 * htmlFS) : parseFloat(1.48 * htmlFS);
    var brandItemH = parseFloat(3.18 * htmlFS);
    var n = brandTop + brandColH - $win.height();
    var inRelate = false,
        pgCFixed = false;
    var that = this;
    var curTop = 0,
        beTop = 0,
        stand = 0;
    that.manji = that.isBrandManji;

    $win.on('scroll.HUB', function() {
        curTop = parseFloat($win.scrollTop());
        if (curTop == 0) { // 显示更多悬浮 隐藏置顶悬浮 隐藏bar 隐藏优惠券 off监听
            that.showMore = true;
            that.goTopShow = false;
            that.scroll = false;
            $doc.off("touchmove.HUB");
            that.inBrand = false;
            that.cfixed = false;
            inRelate = false;
            pgCFixed = false;
            that.manji = that.isBrandManji;
        } else if (curTop > 0 && ((brandTop && curTop < brandTop) || (!brandTop && curTop < relateTop))) { // 隐藏更多悬浮 隐藏置顶悬浮 隐藏bar 隐藏优惠券 off监听
            that.showMore = false;
            that.goTopShow = false;
            that.scroll = false;
            $doc.off("touchmove.HUB");
            that.inBrand = false;
            that.cfixed = false;
            inRelate = false;
            pgCFixed = false;
            that.manji = that.isBrandManji;
        } else if (brandTop && curTop > brandTop && (relateTop && curTop < relateTop) || !relateTop) {
            if (!that.inBrand) { // inBrand 第一次 显示置顶悬浮 显示bar 显示优惠券
                that.inBrand = true;
                that.goTopShow = true;
                that.showMore = false;
                that.scroll = true;
                inRelate = false;
                pgCFixed = false;
                that.manji = that.isBrandManji;
                if (that.isPageBrand) {
                    that.cfixed = false;
                    if(!pgCFixed && curTop >= couponTop) {
                        pgCFixed = true;
                        that.cfixed = that.isBrandManji;
                    }
                } else {
                    that.cfixed = that.isBrandManji;
                }

                that.barTitle = that.brandTitle;
                if (that.brandNum > 20) {
                    stand = Math.ceil((curTop - n) / brandItemH);
                    that.divide = stand > that.brandNum ? that.brandNum : stand;
                }
            } else { // 向上/向下 计算滑动个数
                if (that.isPageBrand && !pgCFixed && curTop >= couponTop) {
                    pgCFixed = true;
                    that.cfixed = that.isBrandManji;
                }

                if (pgCFixed && curTop < couponTop) {
                    that.cfixed = false;
                    that.manji = that.isBrandManji;
                    pgCFixed = false;
                }

                if (curTop < beTop && that.down) { // 向上
                    that.down = false;
                } else if (curTop > beTop && !that.down) { // 向下
                    that.down = true;
                }

                beTop = curTop;

                if (that.brandNum > 20) {
                    stand = Math.ceil((curTop - n) / brandItemH);
                    that.divide = stand > that.brandNum ? that.brandNum : stand;
                    $doc.on("touchmove.HUB", function() {
                        stand = Math.ceil((curTop - n) / brandItemH);
                        that.divide = stand > that.brandNum ? that.brandNum : stand;
                    });
                }
            }
        } else if (relateTop && curTop > relateTop && !inRelate) { // inRelate 隐藏优惠券 off监听
            inRelate = true;
            pgCFixed = false;
            that.inBrand = false;
            that.cfixed = false;
            that.showMore = false;
            that.goTopShow = true;
            that.scroll = true;
            that.manji = false;
            $doc.off("touchmove.HUB");
            that.barTitle = "猜你喜欢";
        }
    });
}
```


![](https://ws2.sinaimg.cn/large/006tKfTcgy1g1ez9l6zw2j30c80c274o.jpg)

![](https://ws2.sinaimg.cn/large/006tKfTcgy1g1ez9nxix6j306c03kaa1.jpg)


#### 后期优化
![](https://ws4.sinaimg.cn/large/006tKfTcgy1g1ezax6t2wj30k0032mx5.jpg)

##### 优化scroll性能

- 函数节流
  - 当持续触发事件时，debounce 会合并事件且不会去触发事件，当一定时间内没有触发再这个事件时，才真正去触发事件。
  - 适用于多次提交（commit）的场景，如点击按钮提交发送请求的情况
- 函数防抖
  - 持续触发事件时，throttle 会合并一定时间内的事件，并在该时间结束时真正去触发一次事件。
  - 适用于scroll/resize等场景

- => 节流方案

``` javascript
var didScroll = false;

$(window).scroll(function() {
    didScroll = true;
});

setInterval(function() {
    if ( didScroll ) {
        didScroll = false;
        fun();
    }
}, 250);
```

#####  优化渲染方式

- 当访问scrollWidth系列、clientHeight系列、offsetTop系列、ComputedStyle等属性时，会JS强制重排，导致Style和Layout前移到JS代码执行过程中

- 使用`requestAnimationFrame()`，将以上特殊操作汇集并延迟入队

``` javascript
const newScrollTop = this.getPosition(this.panes[index].$refs.content).top - this.distance

function scrollStep() {
    document.documentElement.scrollTop += 5
    if (document.documentElement.scrollTop < newScrollTop) {
        window.requestAnimationFrame(scrollStep)
    }
}

window.requestAnimationFrame(scrollStep);
```

##### 做位移效果时使用tranform代替top/left/bottom/right
- `top/left/bottom/right`这类属性会影响元素在文档中的布局，可能改变其他元素的位置，引起重排，造成性能开销
- 使用`transform属性（3D/animation）`将元素提升至合成层，省去布局和绘制环节
  - `合成层`： 合成层的位图，会交由 GPU 合成，比 CPU 处理要快。当需要 repaint 时，只需要 repaint 本身，不会影响到其他的层。

### 3. 提醒组件
- `static/super/src/h5/js/common/subscribe-jq.js`

增加callback

``` javascript
 bindSub: function(selector, spmSub, spmCancel, item) {
    Super.H5.Common.SubScribe({
        triggerSelector: selector,
        isNeedLogin: true,
        isToast: true,
        subsrcibeSelectorText: "提醒我",
        toastSubsrcibedVal: "已取消提醒和收藏",
        toastUnsubsrcibeVal: '已设置提醒并收藏成功',
        subCallback: function(itemId) {
            $.getJSON(ajaxAddSubUrl, {
                item_id: itemId,
                t: new Date().getTime()
            }).done(function(res) {
                UBT.track(spmSub);
                if (item) {
                    Vue.set(item, 'isSubscribe', true);
                }
            });
        },
        unSubCallback: function(itemId) {
            $.getJSON(ajaxAddSubUrl, {
                item_id: itemId,
                t: new Date().getTime()
            }).done(function(res) {
                UBT.track(spmCancel);
                if (item) {
                    Vue.set(item, 'isSubscribe', false);
                }
            });
        }

    });
}
```

### 4. iphoneX 1px边框问题 （若遇到相同问题，可进一步公用）


``` css
/*fl hairlines */
.fl-hairlines .border1px{border-width:0.5px;}
.b1px { border: 1px solid #ccc; }
.fl-hairlines .b1px { border-width: 0.7px;}
 @media all and (max-device-width:320px) {
    .fl-hairlines.fl-iphone .b1px {border-width: 0.5px;}
}
```

- 1px在用了hairlines能解决大部分，但在iphoneX的呈现上会出现**上细下粗且有毛边**

- 当前解决方案：针对`iphonex`，去掉原有border，将border写为`:before`/`:after`定位在按钮上，当前测试border宽度宜为`.8px`

``` css
@media only screen and (device-width:375px) and (device-height:812px) and (-webkit-device-pixel-ratio:3) {
	.fl-hairlines.fl-iphone .brand-list .btn:after { content: ""; position: absolute; top: .8px; left: 0; width: 1.4rem; height: .5rem; border: .8px solid #F10C10; border-radius: .25rem; box-sizing: border-box; }
}
```

### 5. iphoneX公用

``` css
@supports (padding-bottom:constant(safe-area-inset-bottom)){
    .nav-foot{padding-bottom:constant(safe-area-inset-bottom);}
}
```


- `static/webapp/js/common/attrsniffer.js`
- `static/webapp/js/base.js`

``` javascript
if (isIos) {
    html.classList.add("fl-ios");
    vArr = appv.match(/OS (\d+)[_\.](?:\d+)[_\.]?(?:\d+)?/);
    ver = parseInt(vArr[1], 10);

    if (ver >= 8) {
        html.classList.add("fl-hairlines");
    }

    if(ratio == 3) {
        if( w = 375 && h == 812) { // iPhone X、iPhone XS
            html.classList.add("fl-ios-bangs", "fl-ios-x");
        } else if ( w = 414 && h == 896) { // iPhone XS Max
            html.classList.add("fl-ios-bangs", "fl-ios-xmax");
        }
    }

    if(ratio == 2 && w == 414 && h == 896) {
        html.classList.add("fl-ios-bangs", "fl-ios-xr");
    }

} else {
    vArr = appv;
    ver = parseInt(ua.substr(ua.indexOf('android') + 8, 3));
    html.classList.add("fl-android");
    
    if (ver < 6) {
        html.classList.add("fl-android-" + ver);
    }
}
```


``` javascript
// iPhone X、iPhone XS
"isIosX": /ip(hone|od|ad)/i.test(navigator.userAgent.toLowerCase()) && window.devicePixelRatio === 3 && window.screen.width === 375 && window.screen.height === 812,

// iPhone XS Max
"isIosXMax": /ip(hone|od|ad)/i.test(navigator.userAgent.toLowerCase()) && window.devicePixelRatio === 3 && window.screen.width === 414 && window.screen.height === 896,

// iPhone XR
"isIosXR": /ip(hone|od|ad)/i.test(navigator.userAgent.toLowerCase()) && window.devicePixelRatio === 2 && window.screen.width === 414 && window.screen.height === 896,

"isIosBangs": function() {
    return Fanli.Utility.isIosX || Fanli.Utility.isIosXMax || Fanli.Utility.isIosXR;
}
```


### 6. 圆角悬浮动效
![](https://ws3.sinaimg.cn/large/006tKfTcgy1g119qw0ovgj30vt04576k.jpg)

ui想要的效果：
- 边上滑边收圆角
- 页面从z轴分为三层：背景图层，页面滚动层，bar层


实现：
- 圆角：切出一行圆角png图
- 背景图做一个div background-image `z-index:-1;position:fixed;`
- bar层做一个div，限制高度，并且background: url(xxx) no-repeat top; 保证从视觉是bar层的背景图是一直在背景图层的
- 券层虽跟bar层在一个平面，但因为独立fixed，所以也加上background


### 7. skeleton骨架屏切换

- vue-router

`this.$route.name`

- Fanli.Utility.getUrlParam(url, name)

`Fanli.Utility.getUrlParam(href, 'isbrand') == 1`


### 8. 滚动加载 async-data
- `static/super/src/h5/css/common/async-data.css`
- `static/super/src/h5/js/common/async-data.vue.js`



---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)