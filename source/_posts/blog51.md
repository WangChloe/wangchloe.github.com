---
title: php小记
categories: 前端札记
tags: php
date: 2017-07-24 14:55:21
updated: 2019-07-24 14:55:21
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---
<!-- MarkdownTOC -->

- 新建页面php代码
- 清楚页面缓存
- 判断为空不显示该段
- rem计算
- 热区
- if语句
- 循环语句
- 限制输出字数
- gaea配置
- ssh跑脚本
	- 修改品牌开始时间
	- 修改店铺优惠券开始时间
	- 修改店铺商品开售时间

<!-- /MarkdownTOC -->


<!-- more -->

## 新建页面php代码

> Vipday不能写为VipDay，内测环境为linux，不能辨别大写字符，页面报错。

```
<?php

class VipdayAction extends WapcommonAction{

    public function index() {
        $this->_assignWapHeaderFooter();
        $this->display();
    }

    public function floorV2(){
        $this->display('index_floor_v2');
    }

}

?>
```

- 常规

```
<?php
    $cssList = array('/static/super-hd-vipday-index-css.css');
    $jsList = array('/static/super-hd-vipday-index-js.js');
    $title = $gaea['page_title'][0]['data1'];
?>
<include file="$header" remswitch="1" remwidth="640" />


yyy
<include file="$footer" />

```

- 使用ls缓存

```
<?php
   $useZepto = false;
   $useVue = true;
   $useLsCache = ture;
?>

<include file="$header" remswitch="1" remwidth="640" />

xxx
<include file="$footerls" />
```

## 清楚页面缓存

``` html
<?php
    C('CSS_JS_TIME', time());
?>
```

## 判断为空不显示该段

```
<notempty name="gaea28001['Take_rebate'][0]['img']">
<div class="rule-container" id="J_rule_container" data-promotion="{$detail.is_promotion}" data-outshopid="{$detail.out_shop_id}" style="background-image:url({$gaea28001['Take_rebate'][0]['img']});background-size:7.5rem auto;">
    <a href="http://super.fanli.com/h5/item/rule" data-spm="super_prodetial.h5.pty-viewdetail~std-35551"></a>
    <div class="fui-grid">
        <p class="fui-span1 rule-title">{$gaea28001['Take_rebate'][0]['data1']}</p>
        <p class="fui-span0 rule-anchor">查看详情</p>
    </div>
</div>
</notempty>
```

## rem计算

```
<p class="fission-diy-links">
    <volist name="gaea.red_packet_hotpic_number" id="vo">
        <?php
        $point = explode(',', $vo['data1']);
        ?>
        <a style="left:<?php echo round($point[0]/100, 2);?>rem; top:<?php echo round($point[1]/100, 2);?>rem; width:<?php echo round($point[2]/100, 2);?>rem; height:<?php echo round($point[3]/100, 2);?>rem;" href="{$vo.link}"></a>
    </volist>
</p>


<img class="brand-repacket-icon" src="{$brand.icons.brand_inner_left_icon.pic_url}" style="width:<?php echo $brand['icons']['brand_inner_left_icon']['width']/100; ?>rem; height:<?php echo $brand['icons']['brand_inner_left_icon']['height']/100; ?>rem;" alt="">
```

## 热区
``` html
<div class="nation-more">
    <volist name="gaea.game_country_hot_link" id="vo">
    <?php
    $position = explode(',', $vo['data1']);
    ?>
    <a href="{$vo.link}" style="left:{$position[0]/100}rem; top:{$position[1]/100}rem; width:{$position[2]/100}rem; height:{$position[3]/100}rem;"></a>
    </volist>
    <volist name="gaea.game_country_hot_img" id="vo">
    <img class="w100 J_lazyimg" src="http://static2.51fanli.net/common/images/loading/spacer.png" data-original="{$vo.img}" />
    </volist>
</div>
```

## if语句
```
<eq name="brand.promotion.0.is_item" value="1">
{/*预计奖励*/}

<else/>
{/*实际奖励*/}
</eq>
```

## 循环语句
```
<div class="swiper-wrapper">
    <volist name="zhiboList[$j]" id="vo">
        <a href="{$vo.link|default='javascript:void(0);'}">{$vo.txt}</a>
    </volist>
</div>
```

## 限制输出字数

```
<!-- 限制输出9个字 -->
<p class="item-title"><?php echo mb_substr($vo['item_title'],0,9); ?></p>

<p class="item-title fui-uti-nowrap-line">${item.item_title|mb_substr=0,9,'utf-8'}</p>
```

## gaea配置

``` html
<p class="fui-span1 rule-title">{$gaea28001['Take_rebate'][0]['data1']}</p>
```


``` html
<!--循环读取-->
<div class="swiper-wrapper">
    <volist name="zhiboList[$j]" id="vo">
        <a href="{$vo.link|default='javascript:void(0);'}">{$vo.txt}</a>
    </volist>
</div>

<!--读取第一个数值-->
<div class="entrance-title">
    <a href="{$gaea.home_zhibo_left.0.link|default='javascript:void(0);'}">{$gaea.home_zhibo_left.0.data1}</a>
</div>
```

``` html
<img class="w100 lazy" src="http://static2.51fanli.net/common/images/loading/spacer.png" data-original="{$gaea['page_h5_item_column'][0]['img']}" />


```

``` html
<img class="w100" style="min-height:<?php $size = explode('*', $gaea['grab_h5_list_banner'][0]['img_size']); echo round($size[1] / 100, 2); ?>rem" src="{$gaea['grab_h5_list_banner'][0]['img']}" alt="" />
```


``` html
<notempty name="gaea.sy_fc">
	<a id="J_drag_trigger" style="top: <?php echo round($gaea['sy_fc'][0]['data2']/100, 2);?>rem; left:<?php echo round($gaea['sy_fc'][0]['data1']/100, 2);?>rem;width:<?php $size = explode('*', $gaea['sy_fc'][0]['img_size']); echo round($size[0]/100,2) ?>rem; height:<?php $size = explode('*', $gaea['sy_fc'][0]['img_size']); echo round($size[1]/100,2) ?>rem;" href="{$gaea['sy_fc'][0]['link']}" target="newwebview" class="icon-drag">
		<img src="{$gaea['sy_fc'][0]['img']}" alt="">
	</a>
</notempty>
```

## ssh跑脚本

### 修改品牌开始时间
/usr/local/redis/bin/redis-cli -h ip地址 -p 6390

hset super:brand:detail:id:2055 list_time 1504162800


### 修改店铺优惠券开始时间

一个品牌内开售店铺的数量一般为一个

hset super:shop:detail:id:318889 start_time 1504164600

### 修改店铺商品开售时间
hset super:item:detail:num_iid:7855297155 list_time 1504164600

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)