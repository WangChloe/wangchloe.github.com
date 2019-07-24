---
title: 九宫格转盘特效
categories: 前端札记
tags: ['特效','js']
date: 2018-03-09 17:26:12
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

 duang duang duang

<!-- more -->

效果如下

![](http://ww4.sinaimg.cn/large/006tNc79gy1g5b26rryxyg30dg0jux6y.gif)

``` html
<div class="turntable">
    <section class="description">
        <h2>SquaredLotteryRoll</h2>
    </section>
    <div class="table-container table-container-1" id="J_lottery_container_1">
        <h3>- 九宫格</h3>
        <table>
            <tr>
                <td class="lottery-unit lottery-unit-0">
                    <div></div>
                </td>
                <td class="lottery-unit lottery-unit-1">
                    <div></div>
                </td>
                <td class="lottery-unit lottery-unit-2">
                    <div></div>
                </td>
            </tr>
            <tr>
                <td class="lottery-unit lottery-unit-7">
                    <div></div>
                </td>
                <td class="lottery-wrap">
                    <div id="J_btn_start_1">点击抽奖</div>
                </td>
                <td class="lottery-unit lottery-unit-3">
                    <div></div>
                </td>
            </tr>
            <tr>
                <td class="lottery-unit lottery-unit-6">
                    <div></div>
                </td>
                <td class="lottery-unit lottery-unit-5">
                    <div></div>
                </td>
                <td class="lottery-unit lottery-unit-4">
                    <div></div>
                </td>
            </tr>
        </table>
    </div>

    <div class="table-container table-container-2" id="J_lottery_container_2">
        <h3>- 十二宫格</h3>
        <table>
            <tr>
                <td class="lottery-unit lottery-unit-0">
                    <div></div>
                </td>
                <td class="lottery-unit lottery-unit-1">
                    <div></div>
                </td>
                <td class="lottery-unit lottery-unit-2">
                    <div></div>
                </td>
                <td class="lottery-unit lottery-unit-3">
                    <div></div>
                </td>
            </tr>
            <tr>
                <td class="lottery-unit lottery-unit-11"">
                    <div></div>
                </td>
                <td class="lottery-wrap" colspan="2" rowspan="2">
                    <div id="J_btn_start_2">点击抽奖</div>
                </td>
                <td class="lottery-unit lottery-unit-4">
                    <div></div>
                </td>
            </tr>
            <tr>
                <td class="lottery-unit lottery-unit-10"">
                    <div></div>
                </td>
                <td class="lottery-unit lottery-unit-5">
                    <div></div>
                </td>
            </tr>
            <tr>
                <td class="lottery-unit lottery-unit-9">
                    <div></div>
                </td>
                <td class="lottery-unit lottery-unit-8">
                    <div></div>
                </td>
                <td class="lottery-unit lottery-unit-7">
                    <div></div>
                </td>
                <td class="lottery-unit lottery-unit-6">
                    <div></div>
                </td>
            </tr>
        </table>
    </div>
</div>
<script>
    (function(){
        // 排序为左上0 右上2 右下4 左下6

        $.squaredLotteryRoll({
            rollWrapSelector: "#J_lottery_container_1",
            prizeIndex: 1, // 中奖序号从0开始 1为row1 col2
            isDefaultClickFn: true,  // 默认点击事件
            rollBtnSelector: "#J_btn_start_1",
            succCallback: function() {
                alert("抽奖成功");
            }
        });


        $("#J_btn_start_2").on("click", function(){
            $("#J_lottery_container_2").addClass("mode-2");

            $.squaredLotteryRoll({
                rollWrapSelector: "#J_lottery_container_2",
                prizeIndex: 6,
                isDefaultClickFn: false,  // 自定义点击事件
            })
        })
    })();
</script>
```

``` javascript
///<reference path="../vendors/jquery/jquery.js" />

/**
 * @file squaredLotteryRoll
 * @import vendors/jquery/jquery-2.1.1.js
 */

/*
##参数

*/

// 九宫格转盘

(function($) {
    $.extend({
        squaredLotteryRoll: function(options) {
            var settings = $.extend(true, {}, {
                speed: 200, //初始转动速度
                times: 0, //转动次数
                cycle: 50, //转动基本次数：即至少需要转动多少次再进入抽奖环节
                prizeIndex: -1, //中奖位置
                actClass: 'act', // 选中添加样式
                unitSelector: ".lottery-unit", // 每个单元格的类名及索引前缀
                rollWrapSelector: "#J_lottery_roll", // 转盘
                rollBtnSelector: "#J_btn_start", // 抽奖按钮
                isDefaultClickFn: true,
                succCallback: $.noop,
            }, options);

            var $rollWrap = $(settings.rollWrapSelector);
            var $rollBtn = $(settings.rollBtnSelector, settings.rollWrapSelector);
            var $units = $(settings.unitSelector, settings.rollWrapSelector);

            var count = 0; //总共有多少个位置
            var index = 0; //当前转动到哪个位置
            var timer = 0;
            var click = false;
            var times = settings.times;
            var speed = settings.speed;
            var prizeIndex = settings.prizeIndex >= 0 ? settings.prizeIndex : 0;
            var timer;

            function setup() {
                if (!$rollWrap.length) {
                    return;
                }

                settings.isDefaultClickFn ? initClick() : init();;
            }

            function initClick() {
                $rollBtn.on("click", function(ev) {
                    ev.preventDefault();

                    if (click) {
                        return;
                    }

                    init();
                    click = true;
                });
            }

            function init() {
                var len = $units.length;

                if (len > 0) {
                    index = len - 1;
                    $rollWrap.find(settings.unitSelector + "-" + index).addClass(settings.actClass);
                    count = $units.length;
                    roll();
                }
            }

            function roll() {
                times++;
                rolling();

                if (times > settings.cycle + 10 && prizeIndex == index) {
                    clearTimeout(timer);
                    times = 0;
                    click = false;
                    setTimeout(function(){
                        settings.succCallback && settings.succCallback();
                    }, 200);
                } else {
                    if (times < settings.cycle) {
                        speed -= 10;
                    } else {
                        if (times > settings.cycle + 10 && (prizeIndex == 0 && index == (count - 1)) || (prizeIndex == index + 1)) {
                            speed += 110;
                        } else {
                            speed += 20;
                        }
                    }

                    speed < 40 && (speed = 40);
                    timer = setTimeout(roll, speed);
                }

            }

            function rolling() {
                $rollWrap.find(settings.unitSelector + "-" + index).removeClass(settings.actClass);
                index++;
                if (index > count - 1) {
                    index = 0;
                }
                $rollWrap.find(settings.unitSelector + "-" + index).addClass(settings.actClass);
            }

            setup();
        }
    });
}(jQuery));
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)