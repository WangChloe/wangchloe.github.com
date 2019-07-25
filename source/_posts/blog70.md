---
title:  安卓弹窗下拉刷新解决方案
categories: 前端札记
tags: ['项目总结', 'js']
date: 2018-01-17 16:19:36
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

🌈 iscroll - 移动端滑动神器

<!-- more -->

``` html
<div id="J_extra_info" class="extra-info">
    <!-- 绑定在外层 -->
    <div class="info-wrapper" id="J_iscroller"> 
        <!-- 作用在内层 -->
        <div class="img-box" id="scroller">
            <img class="w100" src="{$brand.shops.0.extra_detail_img}">
        </div>
    </div>
    <a href="javascript:void(0);" class="btn-close J_btn_close"></a>
</div>
```

``` html
<style>
.extra-dialog-wrapper{background-color:transparent;width:.6rem;overflow:visible;}
.extra-dialog-wrapper .ui-dialog-content{padding:0;}
.extra-dialog-wrapper .extra-info{padding:.2rem 0;background:#fff;max-height:5.42rem;border-radius:.12rem;overflow:hidden;}
.extra-dialog-wrapper .info-wrapper{background-color:#fff;width:6rem;max-height:5.02rem;overflow:hidden;}
.extra-dialog-wrapper .img-box{width:6rem;}
.extra-info .btn-close{position:absolute;bottom:-1.1rem;left:50%;margin-left:-.3rem;width:.6rem;height:.6rem;background:url(/super/src/h5/images/brand/btn-close-v2.png) no-repeat; background-size:.6rem .6rem;}
</style>
```

``` html
<script>
myScroll = new IScroll("#J_iscroller", {
        scrollX: false,
        scrollY: true
    });

    $extraInfo.dialog({
        width: htmlFS*6,
        autoOpen: false,
        closeBtn: false,
        maskClick: function() {
            $extraInfo.dialog("close");
        },
        open:function(){
            // 解决安卓弹窗下拉刷新问题
            if($window.scrollTop() == 0) {
                window.scrollTo(0,1);              
            }

            // 由于dialog open的时候iscroll不能获取iscroll内宽高，应在open时刷新iscroll重新获取宽高
            myScroll.refresh();
        }
    });

    $extraInfo.closest(".ui-dialog").addClass("extra-dialog-wrapper");

    $extraImg.on("click", function(){
        $extraInfo.dialog("open");
    });
</script>
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)