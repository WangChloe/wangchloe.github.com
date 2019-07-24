---
title: 两种原生组件书写方式
categories: 前端札记
tags: ['项目总结', 'js']
date: 2017-09-24 15:23:37
updated: 2017-09-24 15:23:37
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---
<!-- MarkdownTOC -->

- 第一种：引用执行
- 第二种：将命名空间传递进

<!-- /MarkdownTOC -->


<!-- more -->

> 跨页面，对元素依赖不确定，使用命名空间阻断法传递元素

### 第一种：引用执行
//webapp/js/common/topoverlay.js

``` javascript
<script>
(function(w) {
    var FLNS = {
        "register": function() {
            var a = arguments,
                o = null,
                i, j, d, rt;
            for (i = 0; i < a.length; ++i) {
                d = a[i].split(".");
                rt = d[0];
                eval('if (typeof ' + rt + ' == "undefined"){' + rt + ' = {add: function (k, v) { if (!this[k]) { this[k] = v;} return this;} };} o = ' + rt + ';');
                for (j = 1; j < d.length; ++j) {
                    o[d[j]] = o[d[j]] || {};
                    o = o[d[j]];
                    o.add = function(k, v) {
                        if (!this[k]) { this[k] = v; }
                        return this;
                    };
                }
            }
            return o;
        }
    };

    w.FLNS = FLNS;

})(window);

FLNS.register("NS").add("open", function (options) {
    var settings = $.extend(true, {}, {
        image: {
            ios: "xxx.png",
            android: "yyy.png"
        }
    }, options);

    var elSelector = "#J_weixin_browser";

    function setup() {
        if (!document.querySelector(elSelector)) {
            buildOverlay();
        } else {
            showOverlay();
        }
    }

    function showOverlay() {
        document.querySelector(elSelector).style.display = "block";
    }

    function hideOverlay() {
        document.querySelector(elSelector).style.display = "none";
    }

    function buildOverlay() {
        // code here
    }

    setup();
});
</script>
```

``` javascript
<script>
// 微信引导浮层
function clickBrowser() {
    $btnBrowser.click(function() {
        NS.open({
            image: {
                ios: "zzz.png",
                android: "www.png"
            }
        });
    });
}
</script>
```

### 第二种：将命名空间传递进

//super/src/h5/js/wechat/topoverlay.js

``` javascript
<script>
(function (MODULE) {
    MODULE.add("TopOverlay", function (options) {
        var settings = $.extend(true, {}, {
            autoShow: false,
            showDownload: true,
            image: {
                ios: "xxx.png",
                android: "yyy.png"
            }
        }, options);

        var $window = $(window);
        var $document = $(document);
        var isFromWeixin = wagv && wagv.isFromWeixin;
        var isIos = !!navigator.userAgent.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/);

        var OLSB = new StringBuilder();
        var topoverlaySelector = "#J_wechat_topoverlay";

        function buildOverlay() {
            var imgSrc = isIos ? settings.image.ios : settings.image.android;
             // ...
        }

        function bindAutoShow() {
            if (settings.autoShow) {
                showOverlay();
            }
        }

        function bindLiveEvent() {
            $document.on("click", "{0} .J_close".format(topoverlaySelector), function () {
                hideOverlay();
            });

            $document.on("click", "a", function (ev) {
                var $current = $(this);
                /*
                    1. a 
                    2. a data-superlink="" href="javascript:void(0);" 有个动态跳转

                    鉴于第2点，有优先级关系，将data-superlink嗅探提前
                */
                var href = $current.attr("data-superlink") || $current.attr("data-href") || $current.attr("href");

                if (!href) { return; }

                if (href.indexOf("goshop") > -1 || href.indexOf("taobao.com") > -1) {
                    ev.preventDefault();
                    ev.stopImmediatePropagation();
                    showOverlay();
                    return false;
                }
            });
        }

        function scrollHander() {
            $window.on("scroll.TOPOVERLAY", function () {
                if ($window.scrollTop() > 10) {
                    hideOverlay();
                }
            });
        }

        function showOverlay() {
            $(topoverlaySelector).length ? $(topoverlaySelector).show() : $("body").append(OLSB.toString());
        }

        function hideOverlay() {
            $(topoverlaySelector).hide();
        }

        function setup() {
            buildOverlay();
            bindAutoShow();
            scrollHander();
            bindLiveEvent();
        }

        isFromWeixin ? setup() : "";
    });

}(FLNS.register('NS')));
</script>
```

``` javascript
<script>
    Super.H5.Wechat.TopOverlay();
</script>
```

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)