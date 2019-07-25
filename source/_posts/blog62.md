---
title: fixed input输入框解决方案
categories: 前端札记
tags: ['js', '填坑记']
date: 2018-10-25 11:41:07

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

挖坑填坑～

<!-- more -->

``` html
<style>
/*解决ios-fixed导致间隔*/
body.full-body{position:fixed;top:0;right:0;bottom:0;left:0;width:100%;height:100%;padding:0;margin:0;/*overflow:hidden;*/}
body{width:100%;overflow:visible;}
body.full-body .pop-change{position:absolute;z-index:1100;overflow-x:hidden}
</style>

<div id="J_pop_change" class="fmu-dl-wrap pop-change">
    <h4>修改联系方式</h4>
    <p class="change-info">为保证您中奖后第一时间通知您，请确认您的手机号码</p>
    <div class="tel-wrapper fui-grid">
        <label for="J_tel_change" class="fui-span0">手机号</label>
        <input id="J_tel_change" type="tel" name="number" value="{$mobile}" class="tel-input fui-span1" maxlength="11" placeholder="请输入你的手机号码" />
    </div>
    <div class="code-wrapper">
        <div class="fui-grid code-box">
            <label for="J_msg_code" class="fui-span0">校验码</label>
            <input type="tel" id="J_msg_code" maxlength="6" class="code-input J_required_info msg-code fui-span1" name="code" placeholder="请输入校验码">
        </div>
        <input id="J_get_code" type="button" value="获取校验码" class="btn-send btn-dis" disabled>
    </div>
    <p id="J_change_error" class="error-info">请输入正确的手机号</p>
    <div id="J_change_close" class="btn-submit btn-dis" disabled>确定</div>
</div>
```

``` javascript
// 解决ios-fixed导致间隔
$("#J_tel_change, #J_msg_code").on("focus", function() {
    $body.addClass(iosFixedClass);
    $popChange.css("position", 'absolute');

    popChangeTop();

    if (!isIos) {
        setTimeout(function() {
            $win.on("resize.POPCHANGE", function() {
                popChangeTop();
            });
        }, 300);
    }
});

$("#J_tel_change, #J_msg_code").on("blur", function() {
    $body.removeClass(iosFixedClass);
    $popChange.css("position", 'fixed').css("top", "auto").css("bottom", 0);
    popChangeTop();

    if (!isIos) {
        $win.off("resize.POPCHANGE");
    }
});

function popChangeTop() {
    setTimeout(function() {
        var winH = $win.height();
        var objH = $popChange.height() + 0.3 * htmlFS;
        $popChange.css("top", winH - objH);
    }, 200);
}
```



---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)