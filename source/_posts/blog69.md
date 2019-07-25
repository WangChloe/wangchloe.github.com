---
title: 多倍屏、刘海屏解决方案
categories: 前端札记
tags: ['css', '项目总结']
date: 2019-04-15 16:02:35
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

🙉极客到底！

<!-- more -->

### 多倍屏、刘海屏判断逻辑

``` javascript
(function() {
    /**
     * 一般UA
     * User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 6_1_4 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) Version/6.0 Mobile/10B350 Safari/8536.25 
     * ua has 'context:com', fefine meta viewport width
     */
    var ua = navigator.userAgent.toLowerCase();
    var appv = navigator.appVersion;
    var html = document.documentElement;
    var isIos = /ip(hone|od|ad)/i.test(ua);

    var uaContextReg = /context\:([^\s]+)/i;
    var tryToGetContext = uaContextReg.exec(ua);

    var w = window.screen.width;
    var h = window.screen.height;
    var ratio = window.devicePixelRatio ? window.devicePixelRatio : 0;

    var vArr, ver;
    var dev;

    if (isIos) {
        html.classList.add("ns-ios");
        vArr = appv.match(/OS (\d+)[_\.](?:\d+)[_\.]?(?:\d+)?/);
        ver = parseInt(vArr[1], 10);

        if (ver >= 8) {
            html.classList.add("ns-hairlines");
        }

        if(ratio == 3) {
            if( w = 375 && h == 812) { // iPhone X、iPhone XS
                html.classList.add("ns-ios-bangs", "ns-ios-x");
            } else if ( w = 414 && h == 896) { // iPhone XS Max
                html.classList.add("ns-ios-bangs", "ns-ios-xmax");
            }
        }

        if(ratio == 2 && w == 414 && h == 896) {
            html.classList.add("ns-ios-bangs", "ns-ios-xr");
        }

    } else {
        vArr = appv;
        ver = parseInt(ua.substr(ua.indexOf('android') + 8, 3));
        html.classList.add("ns-android");
        
        if (ver < 6) {
            html.classList.add("ns-android-" + ver);
        }
    }

    if (tryToGetContext && tryToGetContext[1] == 'com') {
        document.querySelector('meta[name=viewport]')
            .setAttribute('content', 'initial-scale=1.0,user-scalable=no,minimum-scale=1.0,maximum-scale=1.0');
    }

}());
```


``` javascript
// iPhone X、iPhone XS
"isIosX": /ip(hone|od|ad)/i.test(navigator.userAgent.toLowerCase()) && window.devicePixelRatio === 3 && window.screen.width === 375 && window.screen.height === 812,

// iPhone XS Max
"isIosXMax": /ip(hone|od|ad)/i.test(navigator.userAgent.toLowerCase()) && window.devicePixelRatio === 3 && window.screen.width === 414 && window.screen.height === 896,

// iPhone XR
"isIosXR": /ip(hone|od|ad)/i.test(navigator.userAgent.toLowerCase()) && window.devicePixelRatio === 2 && window.screen.width === 414 && window.screen.height === 896,

"isIosBangs": function() {
    return isIosX || isIosXMax || isIosXR;
}
```

#### 刘海屏安全距离
``` css
.ns-ios-bangs .video-info { bottom: calc(1.28rem + env(safe-area-inset-bottom)); }
```

``` css
.tuan-tabs{position:fixed; bottom:0; left:0; width:100%; background:#fff; z-index:3;}
@supports (padding-bottom:constant(safe-area-inset-bottom)){
    .tuan-tabs{padding-bottom:constant(safe-area-inset-bottom);}} 
@supports (padding-bottom:env(safe-area-inset-bottom)){
    .tuan-tabs{padding-bottom:env(safe-area-inset-bottom);}
}

/* ios11以下 */
@supports (padding-bottom:constant(safe-area-inset-bottom)) {
    .detail-tab, .detail-buy-panel { padding-bottom: constant(safe-area-inset-bottom); }
    .detail-buy-panel {max-height: calc(100% - 2.14rem - 0.6rem); background:#fff;}
    .detail-buy-panel .panel-select {max-height: calc(100vh - 5.22rem - 0.68rem);}
}

/* ios11以上 */
@supports (padding-bottom:env(safe-area-inset-bottom)) {
    .detail-tab, .detail-buy-panel { padding-bottom: env(safe-area-inset-bottom); }
    .detail-buy-panel {max-height: calc(100% - 2.14rem - 0.6rem); background:#fff;}
    .detail-buy-panel .panel-select {max-height: calc(100vh - 5.22rem - 0.68rem);}
}

```

#### ios 1px解决方案

``` css
.item {
    border: 1px solid #ccc;
}

.ns-hairlines .item {
    border-width: .7px;
}

@media all and (max-device-width:320px) {
    .ns-hairlines .item {
        border-width: .5px;
    }
}
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)