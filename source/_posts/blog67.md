---
title: 刮刮卡特效
categories: 前端札记
tags: ['特效', 'js']
date: 2018-02-25 15:41:40
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

自制大奖


<!-- more -->

``` css
.scratch-card-inner,
.scratch-canvas-wrap { position: relative; width: 6.56rem; height: 2.4rem; }
.scratch-card-wrap .redpacket-info { display: none; margin-top: .32rem; height: .45rem; line-height: .45rem; font-size: .32rem; color: #fff; text-align: center; font-weight: bold; cursor: pointer; }
.scratch-canvas-wrap canvas { z-index: 2; position: absolute; top: 0; left: 0; width: 100%; height: 100%; }
.scratch-card-mask { z-index: 3; position: absolute; top: 0; left: 0; width: 100%; height: 100%; background-image: url(../images/welfare/bg1.jpg); background-size: contain; }
.scratch-card-result { z-index: 1; position: absolute; top: 0; left: 0; width: 100%; height: 100%; color: #cd302b; text-align: center; background: url(../images/welfare/bg2.jpg); background-size: contain; font-weight: bold; }
.scratch-card-result .result-text { margin-top: .26rem; height: .38rem; line-height: .38rem; font-size: .35rem; }
.scratch-card-result .result-award { margin-top: .22rem; height: .85rem; line-height: .85rem; font-size: .65rem; }
.scratch-card-result .result-award i { font-size: .52rem; font-style: normal; vertical-align: -.06rem; }
.scratch-card-result .result-award span { font-size: 1.1rem; vertical-align: -.08rem; }
.scratch-card-result .result-sub-text { margin-top: .14rem; height: .26rem; line-height: .26rem; font-size: .22rem; }
```

``` html
<div class="scratch-card-inner">
    <div id="J_scratch_mask" class="scratch-card-mask"></div>
    <div class="scratch-canvas-wrap">
        <canvas id="J_canvas"></canvas>
        <div id="J_scratch_result" class="scratch-card-result">
            <p class="result-text">恭喜你获得</p>
            <p class="result-award"><i>&yen;</i><span id="J_result_award"></span>无门槛红包</p>
            <p class="result-sub-text">(请在24小时内使用)</p>
        </div>
    </div>
</div>
```

``` javascript
    var ajaxInitCardUrl = "../ajaxGetMyRedPacket";
    var ajaxScratchCardUrl = "../ajaxGetRedPacket";
    var $scratchMask = $("#J_scratch_mask");
    var $resultWrap = $("#J_scratch_result");
    var $resultAward = $("#J_result_award");
    var $canvas = $("#J_canvas");
    var $redPacketInfo = $("#J_redpacket_info");
    var context, canvasW, canvasH, canvas;
    var isFingerOn = false;

    function setup() {
        bindScratchCard();
    }

    function bindScratchCard() {
        $scratchMask.on("click", function(){
            $scratchMask.hide();
        });

        if (!$canvas.length) {
            return;
        }

        canvas = document.getElementById("J_canvas");
        context = canvas.getContext('2d');
        canvasW = $canvas.parent().width();
        canvasH = $canvas.parent().height();

        buildCard();
    }

    function buildCard() {
        initCanvasLayout();
        bindDrawCanvas();
        buildCanvasContent();
        bindCanvasEvent();
    }

    function initCanvasLayout() {
        $canvas.attr("width", canvasW).attr("height", canvasH).css("background-color", "transparent");
    }

    function buildCanvasContent() {
        // var image = new Image();
        // image.crossOrigin = "anonymous";
        // image.src = "../bg1.jpg";
        // context.beginPath();
        // image.onload = function() {
        //     context.drawImage(image, 0, 0, canvasW, canvasH);
        //     context.closePath();
        // }
    }

    function bindDrawCanvas() {
        context.beginPath();
        context.fillStyle = "#CDCBCC";
        context.fillRect(0, 0, canvasW, canvasH);
        context.closePath();
    }

    function bindCanvasEvent() {
        canvas.addEventListener('touchstart', touchStart, false);
        canvas.addEventListener('touchmove', touchMove, false);
        canvas.addEventListener('touchend', touchEnd, false);
    }

    function touchStart(ev) {
        ev.preventDefault();

        var touch = ev.touches[0];
        isFingerOn = true;
    }

    function touchMove(ev) {
        ev.preventDefault();

        var touch = ev.touches[0];
        var canvasPosition = $canvas.offset();
        var canvasLeft = canvasPosition.left;
        var canvasTop = canvasPosition.top;
        var startX = (touch.pageX - canvasLeft);
        var startY = (touch.pageY - canvasTop);

        if (isFingerOn) {
            drawArc(startX, startY, 20);
        }
    }

    function touchEnd(ev) {
        var num = 0;
        var data = context.getImageData(0, 0, canvasW, canvasH);
        isFingerOn = false;
        for (var i = 0; i < data.data.length; i++) {
            if (data.data[i] == 0) {
                num++;
            };
        };

        if (num >= data.data.length * 0.15) {
            context.fillRect(0, 0, canvasW, canvasH);
            $resultWrap.css("z-index", "4");
            $redPacketInfo.css('display', 'block');
        };
    }

    function drawArc(x, y, r) {
        context.globalCompositeOperation = "destination-out";
        context.beginPath();
        context.arc(x, y, r, 0, Math.PI * 2);
        canvas.offsetHeight;
        context.fillStyle = "#000";
        context.fill();
        context.closePath();
    }
```

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)