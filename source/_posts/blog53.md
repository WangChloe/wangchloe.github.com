---
title: 淘口令分享
categories: 前端札记
tags: 项目总结
date: 2017-08-05 15:11:14
updated: 2019-07-24 15:11:14
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---


<!-- MarkdownTOC -->

- 前置需求
- 复制问题
	- 插件：Clipboard.js
	- v1.0
	- v2.0
	- v3.0
	- v4.0
- 反馈
	- 集成smartcopy.js

<!-- /MarkdownTOC -->

<!-- more -->


## 前置需求

- 微信搜索详情页 淘口令弹窗 (ios10与ios10- & android逻辑不同)
"//common/plugins/clipboard/clipboard.min.js",

- ios10- & android  只能长按复制

- ios10 可以按钮一键复制

``` html
<!-- dialog start -->
<div id="J_code_dialog" class="pop-wrap">
    <div class="pop-box" id="J_ready">
        <a id="J_dig_close" class="close ui-dialog-close" href="javascript:void(0);">&times;</a>
        <img class="top" src="//static2.51fanli.net/super/src/h5/images/waitui/detail/dialog-title.png" />
        <p id="J_com" class="com">请一键复制下方特殊符号，打开淘宝APP<br/>即可购买本商品</p>
        <div class="code-wrap">
            <p id="J_item_ios" class="com-code" style="display:none;" ></p>
            <textarea id="J_item_code" class="com-code" value="" type="text"></textarea>
        </div>
        <button id="J_copy_btn" class="copy-btn">一键复制</button>
        <p class="go-ali"><span id="J_go_ali"></span>后，打开［手机淘宝］领取xxx元优惠券！</p>
    </div>
    </div>
</div>
<!-- dialog end -->
<div id="J_taodialog" class="taodilog"></div>
```

``` javascript
<script>
var isIos = /ip(hone|od|ad)/i.test(ua);
var tryUa210 = ua.match(/iphone os (\d+)_/i);
// 淘口令
function popClick($wrap) {
    $wrap.on("click", ".J_item", function() {
        dataWord = $(this).data("word");
        pid = $(this).data("pid");
        $("#J_item_code").val(dataWord);
        $("#J_item_ios").text(dataWord);
        $dialog.show();
        $("#J_taodialog").show();

        if (tryUa210 && tryUa210[1] == 10) {
            $("#J_code_dialog").addClass("ios10");
        };
    });

    $("#J_dig_close").click(function() {
        $("#J_taodialog").hide();
        $dialog.hide();
    });
    $("#J_taodialog").click(function() {
        $("#J_taodialog").hide();
        $dialog.hide();
    });

}


function bindClipboard() { //复制淘口令

    $("#J_item_code").on("input", function() {
        $(this).val(dataWord);
    });

    selectionIos();

    if (tryUa210 && tryUa210[1] == 10) {
        $("#J_com").css("visibility", "hidden");
        $("#J_go_ali").html("复制");
        $("#J_item_code").show().addClass("none");
        var clipboard = new Clipboard("#J_copy_btn", {
            target: function() {
                return document.querySelector("#J_item_code");
            }
        });

        clipboard.on('success', function(e) {
            e.trigger.innerHTML = "已复制";
            $("#J_ready").addClass("ready");
            e.clearSelection();
            UBT.track("super_tuiguang_mj.h5.pty-copy_kl~pid-{0}~std-29412".format(pid));
        });
        clipboard.on('error', function(e) {
            Toast.open("复制失败，请长按后复制特殊符号！");
        });
    } else {
        $("#J_copy_btn").remove();
        $("#J_item_code").addClass("radius-all");
        $("#J_item_ios").addClass("radius-all");

    }
}

function selectionIos() {
    if (!isIos) {
        $("#J_com").html("长按框内>复制");
        $("#J_go_ali").html("复制");
        return;
    }
    $("#J_item_ios").show();
    $("#J_item_code").hide();
    $("#J_com").html("长按框内>拷贝");
    $("#J_go_ali").html("拷贝");
    document.addEventListener("selectionchange", function(e) {
        if (document.getElementById('J_item_ios').innerText != window.getSelection()) {
            var key = document.getElementById('J_item_ios');
            window.getSelection().selectAllChildren(key);
        }
    }, false);
}
</script>
```

## 复制问题

### 插件：Clipboard.js

- `document.queryCommandSupported(copy/cut)`

  - 支持   -> 一键复制

  - 不支持 -> 长按复制
  	- 目前测试的不支持版本：ios10-

### v1.0 

``` html
<div class="code-wrap">
	<p id="J_copy_text" class="copy-text">《mxoy03LIzbI《</p>
</div>
```

- 一键复制问题：

安卓不能获取`<p></p>`标签的复制文本，只能自行长按复制。

- 解决方案：

`<p>` -> `<textarea>`

``` html
<div class="code-wrap">
	<!-- 长按复制 -->
	<p id="J_item_ios" class="copy-code code-ios" style="display:none;">《GjSe0euD5uG《</p>
	<!-- 一键复制 -->
	<textarea id="J_copy_code" class="copy-code" spellcheck="false" value="" type="text">《GjSe0euD5uG《</textarea>
</div>
```

### v2.0

- 一键复制问题：

`<textarea></textarea>`用户可自行修改复制文本。

- 解决方案：

需禁止用户修改输入框，js强制控制。

```
<script type="text/javascript">
	$copyCode.on("input", function() {
        $(this).val(text);
    });
</script>
```

### v3.0

- 长按复制问题：

用户自行复制，较长文本可能漏了一截没有复制，用户体验差。

- 解决方案：

监听selectionchange事件，若选中的与需复制文本不符，则强制window.getSelection().selectAllChildren()复制全部文本。

```
<script type="text/javascript">
    function selectionIos() {
        if (!Device.Utility.Browser.versions.ios) {
            return;
        }

        document.addEventListener("selectionchange", function(e) {
            if (document.getElementById('J_item_ios').innerText != window.getSelection()) {
                var key = document.getElementById('J_item_ios');
                window.getSelection().selectAllChildren(key);
            }
        }, false);
    }
</script>
```

### v4.0

长按复制问题：

- iphone5s测试时，由于rem导致的小屏手机复制文本偏小，可摁区域有限，长按复制经常点不到可复制区域。

- 经ui同意调整字号，使用户体验更佳。


# 反馈

- settings 动态html ✓
- selector优化 ✓
- 多个selectionchange销毁 ✓
- 一键复制定制按钮点击


### 集成smartcopy.js

``` html
<section class="smartcopy-wrapper J_smartcopy_wrapper">
    <h4>复制1(自定义模版)</h4>
    <p id="J_copy_type" class="copy-type"></p>
    <p class="J_press_copy press-copy">￥DADoafiFojoY￥</p>
        <div class="J_click_copy">
        <textarea class="J_copy_code" spellcheck="false" type="text" readonly="readonly">￥DADoafiFojoY￥</textarea>
        <button class="J_copy_btn fui-btn fui-btn-s fui-btn-green">复制</button>
        </div>
</section>

<section class="smartcopy-wrapper J_smartcopy_wrapper2">
    <h4>复制2(默认模版)</h4>
</section>

<script>
    $.smartcopy({
        onCopySuccess: function(){  // 复制成功
            alert("复制成功！");
        }, 
        onCopyFail: function(){  // 复制失败
            alert("复制失败！");
        }, 
        supportCallback: function(){  //支持复制功能回调
            $("#J_copy_type").text("复制方式：支持一键复制，请点击复制按钮！");
        }, 
        notSupportCallback: function(){  //当不支持复制功能时回调
            $("#J_copy_type").text("复制方式：不支持一键复制，请长按文本框复制！");
        },
        defaultTpl: false
    });

    $.smartcopy({
        copyText: "￥Sjxs0UccWFD￥",  
        copyWrapSelector: ".J_smartcopy_wrapper2",
        onCopySuccess: function(){  // 复制成功
            alert("复制成功！");
        }, 
        onCopyFail: function(){  // 复制失败
            alert("复制失败！");
        }, 
    });
</script>
```


``` javascript
///<reference path="../vendors/jquery/jquery.js" />
///<reference path="//webapp/js/base.js"" />
///<reference path="../core/fmu.js" />
///<reference path="../vendors/clipboard/clipboard.min.js" />

/**
 * @file smartcopy
 * @import core/fmu.js, vendors/jquery/jquery-2.1.1.js,vendors/clipboard/clipboard.min.js
 */

/*
##参数
   *copyWrapSelector: ".J_smartcopy_wrapper",  // 多个一键复制框时需定义不同的wrap
    copyCodeSelector: ".J_copy_code", // 待复制文本     
    clickCopySelector: ".J_click_copy",  // 一键复制
    copyBtnSelector: ".J_copy_btn",  // 一键复制按钮
    pressCopySelector: ".J_press_copy",  // 长按复制
    onCopySuccess: $.noop, // 复制成功回调
    onCopyFail: $.noop, //复制失败回调
    supportCallback: $.noop, //支持复制功能回调
    notSupportCallback: $.noop, // 不支持复制功能时回调
    defaultTpl: true, // 使用默认模版
   *copyText: "",  // 使用默认模块时需传入待复制文本内容
*/ 


 (function ($) {
    $.extend({
        smartcopy: function (options) {

            var settings = $.extend(true, {}, {
                copyWrapSelector: ".J_smartcopy_wrapper",
                copyCodeSelector: ".J_copy_code",
                clickCopySelector: ".J_click_copy",
                copyBtnSelector: ".J_copy_btn",
                pressCopySelector: ".J_press_copy",
                onCopySuccess: $.noop,
                onCopyFail: $.noop,
                supportCallback: $.noop,
                notSupportCallback: $.noop,
                defaultTpl: true,
                copyText: "",
            }, options);

            var $scTpl = $('<p class="J_press_copy press-copy">{0}</p>\
                <div class="J_click_copy">\
                <textarea class="J_copy_code" spellcheck="false" type="text" readonly="readonly">{0}</textarea>\
                <button class="J_copy_btn fui-btn fui-btn-s fui-btn-green">复制</button>\
                </div>'.format(settings.copyText));

            var $copyWrap = $(settings.copyWrapSelector);
            var pressCopySelector =  settings.copyWrapSelector + ' ' + settings.pressCopySelector;
            var copyBtnSelector = settings.copyWrapSelector + ' ' + settings.copyBtnSelector;
            var copyCodeSelector = settings.copyWrapSelector + ' ' + settings.copyCodeSelector;

            var $clickCopy, $pressCopy;
            var isSelected = false;

            function setup() {
                initClipboard();
                bindClipboard();
            }

            function initClipboard() {
                settings.defaultTpl && $scTpl.appendTo($copyWrap);

                $clickCopy = $(settings.clickCopySelector, settings.copyWrapSelector);
                $pressCopy = $(settings.pressCopySelector, settings.copyWrapSelector);
            }

            function bindClipboard() {
                if (Clipboard.isSupported()) {  // 一键复制
                    $clickCopy.show();
                    $pressCopy.hide();

                    settings.supportCallback();

                    var clipboard = new Clipboard(copyBtnSelector, {
                        target: function () {
                            return document.querySelector(copyCodeSelector);
                        }
                    });

                    console.log(clipboard);

                    clipboard.on('success', function (e) {
                        settings.onCopySuccess();
                    });

                    clipboard.on('error', function (e) {
                        settings.onCopyFail();
                    });
                } else {  //长按复制(不支持多个)
                    settings.notSupportCallback();
                    selectionIos();
                }
            }

            function selectionIos() {
                if (!Device.Utility.Browser.versions.ios) { 
                    settings.onCopyFail();
                    return;
                }

                $clickCopy.hide();
                $pressCopy.show();

                document.removeEventListener("selectionchange", selectEvent, false);
                document.addEventListener("selectionchange", selectEvent, false);
            }

            function selectEvent(e) {
                if(isSelected) {return;}

                if (document.querySelector(pressCopySelector).innerText != window.getSelection()) {
                    var key = document.querySelector(pressCopySelector);
                    window.getSelection().selectAllChildren(key);
                    isSelected = true;
                }
            }

            setup();
        }
    });
}(jQuery));
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)