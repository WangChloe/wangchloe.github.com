---
title: 每天10个前端知识点：运动框架应用篇(上)
date: 2017-02-02 03:27:35
categories: 前端札记
tags: [js, 应用]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

{% aplayer "Love Mai" "Nicoleta Matei" "http://ojvx9eehr.bkt.clouddn.com/Nicoleta%20Matei%20-%20Love%20Mail.mp3" %}


---
*本次内容：仿jiaThis分享到、幻灯片、手风琴、多图片中心展开*


---

## 1. 仿jiaThis分享到

[JiaThis - 社会化分享按钮及移动端分享代码提供商！](http://www.jiathis.com/)

<!-- more -->

``` css
<style>
	* {
		margin: 0;
		padding: 0;
	}
	body {
		overflow: hidden;
	}
	#box {
		position: absolute;
		right: -140px;
		top: 50%;
		margin-top: -75px;
		padding: 20px;
		width: 100px;
		height: 150px;
		background: rgba(0, 0, 0, .1);
		color: #fff;
		font-size: 20px;
	}
	#box span {
		position: absolute;
		left: -20px;
		width: 20px;
		height: 60px;
		line-height: 20px;
		background: #fe0;
		color: #fff;
		font-size: 12px;
		text-align: center;
	}
</style>
```

``` html
	<div id="box">
		<span>分享到</span>
		<p>QQ</p>
		<p>WeChat</p>
		<p>sina</p>
	</div>
```

``` javascript
<script type="text/javascript" src='move.js'></script>
<script type="text/javascript">
	window.onload = function() {
		var oBox = document.getElementById('box');
		var oSpan = oBox.children[0];

		oBox.onmouseover = function() {
			move(oBox, {right: 0}, {duration: 300});
		}

		oBox.onmouseout = function() {
			move(oBox, {right: -140}, {duration: 300});
		}
	}
</script>
```

效果示例
![move框架应用 - 仿jiaThis分享到](http://ojvx9eehr.bkt.clouddn.com/jiaThis%E5%88%86%E4%BA%AB%E5%88%B0.gif)

## 2. 幻灯片

``` css
<style>
	* {
		margin: 0;
		padding: 0;
	}
	#box {
		position: relative;
		margin: 50px auto;
		width: 400px;
		height: 300px;
		overflow: hidden;
	}
	ul {
		position: absolute;
		top: 0;
		left: 0;
		width: 1200px;
		height: 250px;
	}
	ul li {
		width: 400px;
		height: 250px;
		float: left;
		list-style: none;
	}
	ul li img {
		width: 100%;
		height: 100%;
	}
	ol {
		position: absolute;
		left: 50%;
		bottom: 50px;
		margin-left: -45px;
	}
	ol li {
		margin: 10px;
		width: 10px;
		height: 10px;
		border-radius: 50%;
		border: 1px solid #fff;
		font-size: 8px;
		text-align: center;
		line-height: 10px;
		color: #fff;
		cursor: pointer;
	}
	ol li.active {
		background: #ccc;
	}
</style>
```

``` html
	<div id="box">
		<ul>
			<li><img src="img/slide1.jpg"></li>
			<li><img src="img/slide2.jpg"></li>
			<li><img src="img/slide3.jpg"></li>
		</ul>
		<ol>
			<li class="active">1</li>
			<li>2</li>
			<li>3</li>
		</ol>
	</div>
```

``` javascript
<script type="text/javascript" src='move.js'></script>
<script type="text/javascript">
	window.onload = function() {
		var oBox = document.body.children[0];
		var oUl = oBox.children[0];
		var oOl = oBox.children[1];
		var aLi = oUl.children;
		var aLi2 = oOl.children;

		for(var i = 0; i < aLi2.length; i++) {
			aLi2[i].index = i;
			(function(index) {
				aLi2[i].onmouseover = function() {
					for(var i = 0; i < aLi2.length; i++) {
						aLi2[i].className = '';
					}
					this.className = 'active';
					move(oUl, {marginLeft: - this.index * aLi[0].offsetWidth}, {duration: 500});
				}
			})(i);
		}
	}
</script>
```

效果示例
![move框架应用 - 幻灯片](http://ojvx9eehr.bkt.clouddn.com/move%E6%A1%86%E6%9E%B6%E5%BA%94%E7%94%A8%20-%20%E5%B9%BB%E7%81%AF%E7%89%87.gif)


## 3. 手风琴

``` css
<style>
	* {
		margin: 0;
		padding: 0;
	}
	#box {
		position: relative;
		margin: 50px auto;
		width: 380px;
		height: 200px;
		overflow: hidden;
	}
	#box ul {
		width: 380px;
		height: 200px;
	}
	#box ul li {
		position: absolute;
		list-style: none;
		width: 380px;
		height: 200px;
	}
	#box ul li img{
		width: 100%;
		height: 100%;
	}
</style>
```

``` html
	<div id="box">
		<ul>
			<li><img src="img/slide1.jpg"></li>
			<li><img src="img/slide2.jpg"></li>
			<li><img src="img/slide3.jpg"></li>
			<li><img src="img/slide4.jpg"></li>
			<li><img src="img/slide5.jpg"></li>
		</ul>
	</div>
```

``` javascript
<script type="text/javascript" src='move.js'></script>
<script type="text/javascript">
	window.onload = function() {
		var oBox = document.body.children[0];
		var oUl = oBox.children[0];
		var aLi = oUl.children;

		var w = 30;
		for(var i = 1; i < aLi.length; i++) {
			aLi[i].style.left = oBox.offsetWidth - (aLi.length - i) * w + 'px';
		}

		for(var i = 0; i < aLi.length; i++) {
			aLi[i].index = i;
			aLi[i].onmouseover = function() {
				for(var i = 0; i < aLi.length; i++) {
					if(i <= this.index) {
						move(aLi[i], {left: i * w});  // 小于当前位置往右推
					} else {
						move(aLi[i], {left: oBox.offsetWidth - (aLi.length - i) * w});  // 大于当前位置往左推
					}
				}
			}
		}
		}
</script>
```

效果示例
![move框架应用 - 手风琴](http://ojvx9eehr.bkt.clouddn.com/%E6%89%8B%E9%A3%8E%E7%90%B4.gif)

## 4. 多图片展开

> 这个示例是有问题的，展开的z-index没有调整好，在此求解！

``` css
<style>
	* {
		margin: 0;
		padding: 0;
	}
	#box {
		position: relative;
		margin: 50px auto;
		width: 660px;
	}
	#box ul {
		width: 660px;
	}
	#box ul li {
		float: left;
		list-style: none;
		margin: 10px;
		width: 200px;
		height: 100px;
	}
	#box ul li img {
		width: 100%;
		height: 100%;
	}
</style>
```

``` html
	<div id="box">
		<ul>
			<li><img src="img/slide1.jpg"></li>
			<li><img src="img/slide2.jpg"></li>
			<li><img src="img/slide3.jpg"></li>
			<li><img src="img/slide4.jpg"></li>
			<li><img src="img/slide5.jpg"></li>
			<li><img src="img/slide6.jpg"></li>
			<li><img src="img/slide7.jpg"></li>
			<li><img src="img/slide8.jpg"></li>
			<li><img src="img/slide9.jpg"></li>
		</ul>
	</div>
```

``` javascript
<script type="text/javascript" src='move.js'></script>
<script type="text/javascript">
	window.onload = function() {
		var oBox = document.body.children[0];
		var oUl = oBox.children[0];
		var aLi = oUl.children;
		var bSin = false;

		var aPos = [];
		for(var i = 0; i < aLi.length; i++) {
			aPos[i] = {
				left: aLi[i].offsetLeft,
				top: aLi[i].offsetTop
			}
		}

		//浮动定位 -> 绝对定位
		for(var i = 0; i < aLi.length; i++) {
			aLi[i].style.position = 'absolute';
			aLi[i].style.left = aPos[i].left + 'px';
			aLi[i].style.top = aPos[i].top + 'px';
			aLi[i].style.margin = 0;
		}

		//移上中心放大动画
		for(var i = 0; i < aLi.length; i++) {
			aLi[i].index = i;
			aLi[i].onmouseover = function() {
				if(bSin) return;
				bSin = true;
				for(var i = 0; i < aLi.length; i++) {
					aLi[i].style.zIndex = 0;
				}
				move(this, {
					width: 600,
					height: 300,
					left: 30,
					top: 30
				});
				this.style.zIndex = 1;
			}

			aLi[i].onmouseout = function() {
				move(this, {
					width: 200,
					height: 100,
					left: aPos[this.index].left,
					top: aPos[this.index].top
				});
				bSin = false;
				// this.style.zIndex = 0;
			}
		}
	}
</script>
```

> 这个示例是有问题的，展开的z-index没有调整好，在此求解！

效果示例
![move框架应用 - 多图片展开](http://ojvx9eehr.bkt.clouddn.com/move%E6%A1%86%E6%9E%B6%E5%BA%94%E7%94%A8%20-%20%E5%A4%9A%E5%9B%BE%E7%89%87%E5%B1%95%E5%BC%80.gif)

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)