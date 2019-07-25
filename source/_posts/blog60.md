---
title: 获取淘宝图文详情
categories: 前端札记
tags: ['前端周边', 'js']
date: 2017-11-10 10:37:12
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---


<!-- more -->


``` css
<style>
.detail-box .detail-box-main{overflow:hidden;}
.detail-box-main img{max-width:100%;}
</style>
```

``` html
<div class="J_detail_info">
    <div class="detail-box">
        <div class="detail-box-main" id="J_detail_box_main">
            {/*图文详情*/}
        </div>
    </div>
</div>


<section style="display:none;">
    <input type="hidden" value="{$detail.num_iid}" id="J_item_id">
</section>
```


``` javascript
<script>
// 图文详情
function getDetail(obj) {
    obj.data("lock", "1");
    $.getJSON(detailUrl, { pid: itemId })
        .done(function (res) {
            if (res.status == 1) {
                $picDetailBox.html(res.data.replace(/style=[^\>]*/g, function (style) {
                    return "";
                }).replace(/(height|width)=[\'\"].*?[\'\"]/g, function (style) {
                    return "";
                }).replace(/(class)=[\'\"].*?[\'\"]/g, function (style) {
                    return "";
                }).replace(/src=[\'\"](.*?\.(jpg|gif|png))[\'\"]/g, function (match, p1) {
                    return " src='{0}'".format(p1);
                }));
            }
        });
}
</script>
```

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)