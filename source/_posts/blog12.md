---
title: 每天10个前端知识点：运动框架应用篇(中)
date: 2017-02-03 03:27:35
categories: 前端札记
tags: [js, 应用]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

{% aplayer "Hello" "OMFG" "http://ojvx9eehr.bkt.clouddn.com/OMFG%20-%20Hello.mp3" %}


---
*本次内容：运动时钟、返回顶部、无缝滚动、无缝幻灯片、带进度条的无缝幻灯片*


---

## 5. 运动时钟


<!-- more -->
``` css
<style>
	* {
		margin: 0;
		padding: 0;
	}
	#box {
		margin: 20px auto;
		width: 185px;
		height: 35px;
		line-height: 35px;
		overflow: hidden;
	}
	#box ul li {
		float: left;
		position: relative;
		list-style: none;
		width: 23px;
		height: 35px;
	}
	#box ul li img {
		position: absolute;
	}
</style>
```

``` html
	<div id="box">
		<ul>
			<li><img src="img/num.png"></li>
			<li><img src="img/num.png"></li>
			<li>:</li>
			<li><img src="img/num.png"></li>
			<li><img src="img/num.png"></li>
			<li>:</li>
			<li><img src="img/num.png"></li>
			<li><img src="img/num.png"></li>
		</ul>
	</div>
```
``` javascript
<script type="text/javascript" src='move.js'></script>
<script>
	function toDou(num) {
		return num < 10 ? '0' + num : '' + num;
	}

	window.onload = function() {
		var oBox = document.body.children[0];
		var oUl = oBox.children[0];
		var aImg = oUl.getElementsByTagName('img');

		clock();
		setInterval(clock, 1000);

		function clock() {
			var oDate = new Date();
			var iH = oDate.getHours();
			var iM = oDate.getMinutes();
			var iS = oDate.getSeconds();

			var str = toDou(iH) + toDou(iM) + toDou(iS);

			for(var i = 0; i < aImg.length; i++) {
				// aImg[i].style.top = - str.charAt(i) * 35 + 'px';
				move(aImg[i], {top: - str.charAt(i) * 35}, {duration: 500});
			}
		}
	}
</script>
```


效果示例
![move框架应用 - 动态时钟](http://ojvx9eehr.bkt.clouddn.com/%E5%8A%A8%E6%80%81%E6%97%B6%E9%92%9F.gif)

## 6. 返回顶部

> 此例因为涉及documentElement，move.js中没有相关判断，所以用move.js原理写了一个

``` css
<style>
	body {
		height: 3000px;
	    background: linear-gradient(red,blue);
	}
	input {
	    position: fixed;
	    right: 20px;
	    bottom: 20px;
	    display: none;
	}
</style>
```

``` html
	<input type="button" value="返回顶部" id="btn1" />
```
``` javascript
<script>
	window.onload = function() {
	    var oBtn = document.getElementById('btn1');

	    var timer = null;
	    // 添加滚动事件
	    var bSin = false;
	    window.onscroll = function() {
	        if (bSin) {
	            clearInterval(timer);
	        }
	        bSin = true;
	        var scrollT = document.documentElement.scrollTop || document.body.scrollTop;
	        if (scrollT > 0) {
	            // 按钮出来
	            oBtn.style.display = 'block';
	        } else {
	            oBtn.style.display = 'none';
	        }
	    };

	    // 按钮事件
	    oBtn.onclick = function() {
	        // 先关后开
	        var scrollT = document.documentElement.scrollTop || document.body.scrollTop;
	        clearInterval(timer);
	        var count = Math.floor(1000/30);
	        var dis = 0 - scrollT;
	        var n = 0;
	        timer = setInterval(function(){
	            bSin = false;
	            n++;
	            var a = 1-n/count;
	            var cur = scrollT + dis*(1-Math.pow(a,3));
	            document.documentElement.scrollTop = document.body.scrollTop = cur;
	            if (n == count) {
	                clearInterval(timer);
	            }
	        },30);
	    };
	};
</script>
```

效果示例
![move框架应用 - 返回顶部](http://ojvx9eehr.bkt.clouddn.com/%E8%BF%94%E5%9B%9E%E9%A1%B6%E9%83%A8.gif)

## 7. 无缝滚动

> 此例资源为4张不重复图片，宽度为相应宽度*4

1. ul里的内容复制一份达到无缝的目的，再计算ul宽度

2. **模%求得余数**
  - 往左走(left负数)
	W = oUl.offsetWidth / 2;
	left -= 5;
	left = left % W;
  - 往右走(left正数)
	W = oUl.offsetWidht / 2;
	left += 5;
	left = (left % W - W) % W;

``` css
<style>
	* {
	    margin: 0;
	    padding: 0;
	}
	#box {
	    width: 1280px;
	    height: 220px;
	    border: 1px solid #000;
	    position: relative;
	    margin: 100px auto;
	    overflow: hidden;
	}
	#box ul {
	    position: absolute;
	    left: 0;
	    top: 0;
	}
	#box ul li {
	    float: left;
	    padding: 10px;
	    list-style: none;
	    width: 300px;
	    height: 200px;
	}
	#box ul li img {
	    width: 100%;
	    height: 100%;
	}
	#box span {
	    z-index: 2;
	    position: absolute;
	    top: 0;
	    width: 640px;
	    height: 220px;
	}
	#left {
	    left: 0;
	}
	#right {
	    right: 0;
	}
</style>
```

``` html
	<div id="box">
	    <ul>
	        <li><img src="img/slide1.jpg" alt=""></li>
	        <li><img src="img/slide2.jpg" alt=""></li>
	        <li><img src="img/slide3.jpg" alt=""></li>
	        <li><img src="img/slide4.jpg" alt=""></li>
	    </ul>
	    <span id="left"></span>
	    <span id="right"></span>
	</div>
```
``` javascript
<script>
	window.onload = function() {
	    var oBox = document.getElementById('box');
	    var oUl = oBox.children[0];
	    var oL = oBox.children[1];
	    var oR = oBox.children[2];
	    var aLi = oUl.children;

	    // 内容复制一份达到无缝的目的
	    oUl.innerHTML += oUl.innerHTML;

	    // 重新计算ul宽度
	    oUl.style.width = aLi[0].offsetWidth * aLi.length + 'px';

	    var timer = null;
	    oL.onmouseover = function() {
	        toLeft();
	    };
	    oR.onmouseover = function() {
	        toRight();
	    };

	    var left = 0;
	    var W = oUl.offsetWidth/2;

	    toRight();  // 默认向右滚动

	    function toRight() {
	        clearInterval(timer);
	        timer = setInterval(function(){
	            left += 5;
	            oUl.style.left = (left%W-W)%W + 'px';
	        },30);
	    }

	    function toLeft() {
	        clearInterval(timer);
	        timer = setInterval(function(){
	            left -= 5;
	            oUl.style.left = (left%W-W)%W + 'px';
	        },30);
	    }
	};
</script>
```
效果示例
![move框架应用 - 无缝滚动](http://ojvx9eehr.bkt.clouddn.com/%E6%97%A0%E7%BC%9D%E6%BB%9A%E5%8A%A8.gif)


## 8. 无缝幻灯片

1. ul里的内容复制一份达到无缝的目的，再计算ul宽度

2. **模%求得余数**
  - 往左走(left负数)
	W = oUl.offsetWidth / 2;
	left -= 5;
	left = left % W;
  - 往右走(left正数)
	W = oUl.offsetWidht / 2;
	left += 5;
	left = (left % W - W) % W;

``` css
<style>
	* {
		margin: 0;
		padding: 0;
	}
	#box {
		position: relative;
		margin: 50px auto;
		width: 600px;
		height: 350px;
		overflow: hidden;
	}
	#box ul {
		position: absolute;
	}
	#box ul li {
		float: left;
		list-style: none;
		width: 100%;
		height: 100%;

	}
	#box ul li img {
		width: 100%;
		height: 100%;
	}
	#box ol {
		position: absolute;
		left: 50%;
		bottom: 10px;
		width: 100px;
		margin-left: -50px;
	}
	#box ol li {
		float: left;
		margin: 5px;
		list-style: none;
		width: 10px;
		height: 10px;
		border-radius: 50%;
		background: #666;
		opacity: .7;
		cursor: pointer;
	}
	#box ol li.active {
		background: #fff;
	}
	#box a {
		position: absolute;
		z-index: 1;
		top: 50%;
		margin-top: -25px;
		width: 30px;
		height: 50px;
		line-height: 50px;
		text-align: center;
		background: #666;
		color: #fff;
		opacity: .1;
		border-radius: 2px;
		text-decoration: none;
	}
	#box a.prev {
		left: 0;
	}
	#box a.next {
		right: 0;
	}
</style>
```

``` html
	<div id="box">
		<a href="javascript:;" class="prev">←</a>
		<a href="javascript:;" class="next">→</a>
		<ul>
			<li><img src="img/slide1.jpg"></li>
			<li><img src="img/slide2.jpg"></li>
			<li><img src="img/slide3.jpg"></li>
			<li><img src="img/slide4.jpg"></li>
			<li><img src="img/slide5.jpg"></li>
		</ul>
		<ol>
			<li class="active"></li>
			<li></li>
			<li></li>
			<li></li>
			<li></li>
		</ol>
	</div>
```
``` javascript
<script type="text/javascript" src='move.js'></script>
<script>
	window.onload = window.onresize = function() {
		var oBox = document.body.children[0];
		var oPrev = oBox.children[0];
		var oNext = oBox.children[1];
		var oUl = oBox.children[2];
		var oOl = oBox.children[3];
		var aLi = oUl.children;
		var aBtn = oOl.children;

		var iNow = 0;

		oBox.onmouseover = function() {
			clearInterval(timer2);
			move(oPrev, {opacity: 0.7}, {duration: 500});
			move(oNext, {opacity: 0.7}, {duration: 500});
		}

		oBox.onmouseout = function() {
			carousel();
			move(oPrev, {opacity: 0.1}, {duration: 300});
			move(oNext, {opacity: 0.1}, {duration: 300});
		}

		for(var i = 0; i < aLi.length; i++) {
			aLi[i].style.width = oBox.offsetWidth + 'px';
		}
		oUl.innerHTML += oUl.innerHTML;
		oUl.style.width = aLi.length * (aLi[0].offsetWidth) + 'px';

		var timer2 = null;
		carousel();

		function carousel() {
			clearInterval(timer2);
			timer2 = setInterval(function() {
				iNow++;
				tab();
			}, 3000);
		}

		for(var i = 0; i < aBtn.length; i++) {
			aBtn[i].index = i;
			aBtn[i].onmouseover = function() {
				// iNow = this.index;

				//第几轮，解决第二轮之后上移运动回第一轮的问题
				iNow = Math.floor(iNow/aBtn.length)*aBtn.length+this.index;
				tab();
			}
		}

		oPrev.onclick = function() {
			iNow--;
			tab();
		}

		oNext.onclick = function() {
			iNow++;
			tab();
		}

		function tab() {
			for(var i = 0; i < aBtn.length; i++) {
				aBtn[i].className = '';
			}

			//解决当iNow<0时aBtn[iNow%aBtn.length]找不到的问题
            aBtn[(iNow%aBtn.length+aBtn.length)%aBtn.length].className = 'active';
			// move(oUl, {left: - aLi[0].offsetWidth * iNow}, {duration: 500});
			toR(oUl, - aLi[0].offsetWidth * iNow);
		}

		var iW = oUl.offsetWidth / 2;
		var timer = null;
		var left = 0;
		function toR(obj, iTarget) {
			clearInterval(timer);
			var start = left;
			var dis = iTarget - start;
			var count = Math.floor(1000 / 30);
			var n = 0;
			timer = setInterval(function() {
				n++;
				var a = n / count;
				var cur = start + dis * a;
				left = cur;
				oUl.style.left = (left % iW - iW) % iW + 'px';

				if(n == count) {
					clearInterval(timer);
				}
			}, 30);
		}
	}
</script>
```
效果示例
![move框架应用 - 无缝幻灯片](http://ojvx9eehr.bkt.clouddn.com/%E6%97%A0%E7%BC%9D%E5%B9%BB%E7%81%AF%E7%89%87.gif)

## 9. 带进度条的无缝幻灯片

``` css
<style>
	* {
		margin: 0;
		padding: 0;
	}
	#box {
		position: relative;
		margin: 50px auto;
		width: 350px;
		height: 200px;
		overflow: hidden;
	}
	#box ul {
		position: absolute;
	}
	#box ul li {
		float: left;
		list-style: none;
		width: 350px;
		height: 200px;
		text-align: center;
		line-height: 200px;
		font-size: 80px;
		color: #ffb;
	}
	#box ol {
		position: absolute;
		left: 50%;
		bottom: 10px;
		margin-left: -150px;
		width: 300px;
	}
	#box ol li {
		position: relative;
		float: left;
		margin: 10px;
		list-style: none;
		width: 40px;
		height: 10px;
		background: #fff;
	}
	#box ol li span {
		position: absolute;
		top: 0;
		width: 0;
		height: 10px;
		background: #666;
	}
</style>
```

``` html
	<div id="box">
		<ul>
			<li style="background: #aaa">1</li>
			<li style="background: #afe">2</li>
			<li style="background: #50f">3</li>
			<li style="background: #aea">4</li>
			<li style="background: #fe0">5</li>
		</ul>
		<ol>
			<li><span></span></li>
			<li><span></span></li>
			<li><span></span></li>
			<li><span></span></li>
			<li><span></span></li>
		</ol>
	</div>
```
``` javascript
<script type="text/javascript" src='move.js'></script>
<script>
	window.onload = function() {
		var oBox = document.body.children[0];
		var oUl = oBox.children[0];
		var aLi = oUl.children;
		var oOl = oBox.children[1];
		var aSpan = oOl.getElementsByTagName('span');

		oUl.innerHTML += oUl.innerHTML;
		oUl.style.width = aLi.length * aLi[0].offsetWidth + 'px';

		var iNow = 0;
		next();

		function next() {
			move(aSpan[iNow % aSpan.length], {width: 40}, {easing: 'ease-out',complete: function(){
				// 回调函数
				iNow++;
				for(var i = 0; i < aSpan.length; i++) {
					aSpan[i].style.width = 0;
				}
				move2(oUl, -iNow * aLi[0].offsetWidth, function() {
					!bSin && next();
				});
			}});
		}

		var iW = oUl.offsetWidth / 2;
		var left = 0;
		var timer = null;

		function move2(obj, iTarget, complete) {
			clearInterval(timer);
			var start = left;
			var dis = iTarget - start;
			var count = Math.floor(1000 / 30);
			var n = 0;
			timer = setInterval(function() {
				n++;
				var a = n / count;
				var cur = start + dis * a;
				left = cur;
				obj.style.left = (left % iW - iW) % iW + 'px';
				if(n == count) {
					clearInterval(timer);
					complete && complete();
				}
			}, 30);
		}

		var bSin = false;
		oBox.onmouseover = function() {
			bSin = true;
			for(var i = 0; i < aSpan.length; i++) {
				clearInterval(aSpan[i].timer);
				aSpan[i].style.width = 0;
			}
		}

		oBox.onmouseout = function() {
			bSin = false;
			next();
		}

	}
</script>
```

效果示例
![move框架应用 - 带进度条的无缝幻灯片](http://ojvx9eehr.bkt.clouddn.com/%E5%B8%A6%E8%BF%9B%E5%BA%A6%E6%9D%A1%E7%9A%84%E6%97%A0%E7%BC%9D%E5%B9%BB%E7%81%AF%E7%89%87.gif)

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)