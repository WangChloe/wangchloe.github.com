---
title: 三栏tab横向滑动
categories: 前端札记
tags: ['特效','js']
date: 2017-06-13 11:23:00
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---


<!-- more -->

### 两栏滑动

```
<style>
.detail-all-box{width:100%; -webkit-transition:all .2s ease-in-out; -webkit-transform:translate3d(0,0,0); -webkit-transform-style:preserve-3d; transform-style:preserve-3d; -webkit-backface-visibility:hidden; backface-visibility:hidden; -webkit-perspective:1000; perspective:1000;}

.detail-all-box.act{ -webkit-transform:translate3d(-100%,0,0);}

.commodity{width:7.5rem; overflow:hidden;}

.details{width:7.5rem; overflow:hidden;}
</style>
```

```
<div id="J_detail_tab" class="detail-tab fui-grid">
    <a class="fui-span1 act" href="javascript:void(0);" data-spm="super_mj_tuiguang.h5.pty-tab~id-1~std-30531">商品</a>
    <a class="fui-span1" href="javascript:void(0);" data-spm="super_mj_tuiguang.h5.pty-tab~id-2~std-30531"  data-detail="1">图文详情</a>
</div>
 <div class="detail-wrap">
    <div id="J_detail_all_box" class="detail-all-box fui-grid">
        <div class="J_detail_info commodity">
        </div>
        <div class="J_detail_info details">
        </div>
    </div>
</div>
```

```
<script>
// 顶部选项卡
function bindTabClick() {
    var $detailInfo = $(".J_detail_info");
    $detailTab.on("click", "a", function () {
        var $this = $(this);
        var index = $this.index();
        if (!$this.hasClass("act")) {
            window.scrollTo(0, 0);
            $this.addClass("act").siblings("a").removeClass("act");
            $detailInfo.height($(window).height() - $detailTab.height() * 3);
            if (index) {
                setTimeout(function () {
                    $("#J_detail_all_box").addClass("act");
                }, 10);
            } else {
                setTimeout(function () {
                    $("#J_detail_all_box").removeClass("act");
                }, 10);
            }
            setTimeout(function () {
                $detailInfo.eq(index).css("height", "auto");
            }, 10);
            $this.data("detail") =="1" && $this.data("lock") !=="1" && getDetail($this);
        }
    });
}
</script>
```


### 三栏滑动


```
<style>
.try-container{width:7.5rem;overflow:hidden;}
.apply-ing, .apply-succ, .apply-fail{width:7.5rem;overflow:hidden;}
.try-all-box{width:100%;-webkit-transition:all .2s ease-in-out;-webkit-transform:translate3d(0,0,0);-webkit-transform-style:preserve-3d;transform-style:preserve-3d;-webkit-backface-visibility:hidden;backface-visibility:hidden;-webkit-perspective:1000;perspective:1000;}
.try-all-box.act{-webkit-transform:translate3d(-100%,0,0);}
.try-all-box.act2{-webkit-transform:translate3d(-200%,0,0);}
</style>
```

```
<div id="J_my_try_tab" class="try-tab fui-grid">
    <a class="fui-span1 act" href="javascript:void(0);">申请中</a>
    <a class="fui-span1" href="javascript:void(0);">申请成功</a>
    <a class="fui-span1" href="javascript:void(0);">申请失败</a>
</div>
<div class="try-container">
    <div id="J_my_try_box" class="try-all-box fui-grid">
        <div class="J_my_try_info apply-ing J_apply_ing">
            <include file="inc_apply_ing" />
        </div>
        <div class="J_my_try_info apply-succ">
            <include file="inc_apply_succ" />
        </div>
        <div class="J_my_try_info apply-fail J_apply_fail">
            <include file="inc_apply_fail" />
        </div>
    </div>
</div>
```

```
<script>
    function bindTabClick() {
        var $myTryBox = $("#J_my_try_box");
        var $myTryInfo = $(".J_my_try_info");
        $myTryTab.on("click", "a", function() {
            var $this = $(this);
            var index = $this.index();
            if (!$this.hasClass("act")) {
                window.scrollTo(0, 0);
                $this.addClass("act").siblings("a").removeClass("act");
                $myTryInfo.height($win.height() - $myTryTab.height() * 2);
                if (index) {
                    if (index == 1) {
                        setTimeout(function() {
                            $myTryBox.addClass("act").removeClass("act2");
                        }, 10);
                    } else if (index == 2) {
                        setTimeout(function() {
                            $myTryBox.addClass("act2").removeClass("act");
                        }, 10);
                    }
                } else {
                    setTimeout(function() {
                        $myTryBox.removeClass("act act2");
                    }, 10);
                }
                setTimeout(function() {
                    $myTryInfo.eq(index).css("height", "auto");
                }, 10);
            }
        });
    }
</script>
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)