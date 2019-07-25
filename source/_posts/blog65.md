---
title: 翻牌特效解决方案
categories: 前端札记
tags: ['特效', 'js']
date: 2017-12-08 15:27:55
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

业务场景：卡片自动翻牌，翻牌时替换商品图及链接


<!-- more -->


``` css
.container{width:990px;}
.jx-wrapper .jx-box{float:left;width:322px;height:248px;margin-right:12px;}
.jx-wrapper .jx-box a{display:block;cursor:pointer;}
```

``` html
<div class="container jx-container" id="J_jx_container">
    <img class="lazy" src="//static2.51fanli.net/common/images/loading/spacer.png" data-original="{$gaea['home_jingxuan_pc_column'][0]['img']}">
    <?php
    $jingxuanList = array();
    for ($i = 0; $i < count($gaea['home_pc_jingxuan']); $i++) {
        $jxIdx = (int)$gaea['home_pc_jingxuan'][$i]['data1'];
        if ($jxIdx < 1 || $jxIdx > 3) {
            continue;
        }
        $jingxuanList[$jxIdx][] = array('img'=>$gaea['home_pc_jingxuan'][$i]['img'], 'link'=>$gaea['home_pc_jingxuan'][$i]['link']);
    }
    ksort($jingxuanList);
    $jingxuanList = array_values($jingxuanList);
    ?>
    <div class="jx-wrapper clearfix">
        <volist name="jingxuanList" id="jxvolist">
            <div class="jx-box animated J_jx_box"></div>
        </volist>
    </div>
</div>
```


## v1.0
``` javascript
function bindJxAnimateFlip() {
    var $jxContainer = $("#J_jx_container");
    var $jxInfo = $("#J_jx_info");
    var animationName = "flipInX";
    var round = 0,
    init = true;
    var arrInfo, len, $jxBox;

    if (!$jxContainer.length) {
        return;
    }

    $jxBox = $(".J_jx_box");
    arrInfo = strTrsArr($jxInfo.val());
    len = arrInfo[0].length;

    bindImgSrc();

    setInterval(function() {
        bindImgSrc();
    }, 3000);

    function bindImgSrc() {
        if (round >= len) {
            round = 0;
        }

        $.each($jxBox, function(index, el) {
            var $el = $(el);
            var objInfo = arrInfo[index][round];

            $el.removeClass(animationName);

            if (!init) {
                setTimeout(function() {
                    $el.html('<a href="{0}"></a><img class="w100" src="{1}" />'.format(objInfo.link, objInfo.img));
                    $el.addClass(animationName);
                }, 50);
            } else {
                $el.html('<a href="{0}"></a><img class="w100" src="{1}" />'.format(objInfo.link, objInfo.img));
            }
        });

        round++;
        init = false;
    }

    function strTrsArr(str) {
        var fn = new Function('return' + str);
        return fn();
    }
}
```

## v2.0
``` javascript
function bindJxAnimateFlip() {
    var $jxContainer = $("#J_jx_container");
    var source = JSON.parse($('#J_jx_info').attr('value'));

    $('.J_jx_box').each(function(idx, el) {
        var $this = $(this);
        var s = source[idx];
        var len = s.length;
        var i = 0;
        $this.on('webkitAnimationIteration', function() {
            if(i >= len ){
                i = 0;
            }
            $this.html('<a href="{0}"></a><img class="w100" src="{1}" />'.format(s[i].link, s[i].img));
            i++ ;
        })
    });
}
```

## v3.0
``` javascript
function bindJxAnimateFlip() {
    var $jxContainer = $("#J_jx_container");
    var source = JSON.parse($('#J_jx_info').attr('value'));

    $('.J_jx_box').each(function(idx, el) {
        var $this = $(this);
        var s = source[idx];
        var len = s.length;
        var i = 0;
        var $link = $this.find(".J_jx_link");
        var $img = $this.find(".J_jx_img");

        $this.on('webkitAnimationIteration', function() {
            if(i >= len ){
                i = 0;
            }

            $link.attr("href", s[i].link);
            $img.attr("src", s[i].img);

            i++ ;
        })
    });
}
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)