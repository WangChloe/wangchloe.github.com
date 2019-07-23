---
title: 每天10个前端知识点：运动框架应用篇(下)
date: 2017-02-04 03:27:35
categories: 前端札记
tags: [js, 应用]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。




---
*本次内容：打字依次显示效果、收起当前页放出下一页效果、分块运动、无缝幻灯片、带进度条的无缝幻灯片*


---


## 10. 打字依次显示效果

<!-- more -->


``` css
<style>
	body {
		background: #000;
	}
	span {
		color: #fff;
		font-size: 20px;
		opacity: 0;
	}
</style>
```

``` html

```
``` javascript
<script type="text/javascript" src='move.js'></script>
<script>
	window.onload = function () {
		var str = '往左走(left负数)W = oUl.offsetWidth / 2;left -= 5;left = left % W;往右走(left正数)W = oUl.offsetWidht / 2;left += 5;left = (left % W - W) % W;';
		for(var i = 0; i < str.length; i++) {
			var oS = document.createElement('span');
			oS.innerHTML = str.charAt(i);
			document.body.appendChild(oS);
		}

		var aSpan = document.body.children;
		var i = 0;

		// 分步运动
		var timer = setInterval(function() {
			move(aSpan[i], {opacity: 1}, {duration: 100});
			i++;
			if(i == aSpan.length) {
				clearInterval(timer);
			}
		}, 100);

	}
</script>
```
效果示例
![move框架应用 - 打字依次显示效果](http://ojvx9eehr.bkt.clouddn.com/%E6%89%93%E5%AD%97%E4%BE%9D%E6%AC%A1%E5%BC%B9%E5%87%BA%E6%95%88%E6%9E%9C.gif)


## 11. 收起当前页放出下一页效果

``` css
<style>
	* {
	    margin: 0;
	    padding: 0;
	}
	ul {
	    width: 516px;
	    margin: 50px auto;
	}
	ul li {
	    list-style: none;
	    width: 150px;
	    height: 150px;
	    background: #ccc;
	    float: left;
	    margin: 10px;
	    border: 1px solid #000;
	}
</style>
```

``` html
	<input type="button" value="下一页" id="btn1" />
	<ul id="ul1">
	    <li></li>
	    <li></li>
	    <li></li>
	    <li></li>
	    <li></li>
	    <li></li>
	    <li></li>
	    <li></li>
	    <li></li>
	</ul>
```

``` javascript
<script type="text/javascript" src='move.js'></script>
<script>
	window.onload = function() {
	    var oBtn = document.getElementById('btn1');
	    var oUl = document.getElementById('ul1');
	    var aLi = oUl.children;

	    // 浮动定位 -> 绝对定位
	    var aPos = [];
	    for (var i = 0; i < aLi.length; i++) {
	        aPos[i] = {
	            left: aLi[i].offsetLeft,
	            top: aLi[i].offsetTop
	        };
	    }
	    for (var i = 0; i < aLi.length; i++) {
	        aLi[i].style.position = 'absolute';
	        aLi[i].style.left = aPos[i].left + 'px';
	        aLi[i].style.top = aPos[i].top + 'px';
	        aLi[i].style.margin = 0;
	    }

	    var timer = null;
	    var bSin = false;
	    oBtn.onclick = function() {
	        if (bSin) return;
	        bSin = true;

	        // 分步运动
	        var i = 0;
	        timer = setInterval(function(){
	            (function(index){
	                // 收起
	                move(aLi[i], {
	                    left: 0, top:0, width: 0,
	                    height: 0, opacity: 0
	                },{
	                    complete: function() {	// 回调函数
	                        if (index == aLi.length-1) {
	                            // 放出
	                            for (var i = 0; i < aLi.length; i++) {
	                                aLi[i].style.background = 'rgb('+parseInt(Math.random()*256)+','+parseInt(Math.random()*256)+','+parseInt(Math.random()*256)+')';
	                            }

	                            timer = setInterval(function(){
	                                (function(index2){
	                                    move(aLi[index], {
	                                        left: aPos[index].left, top: aPos[index].top, width: 150, height: 150, opacity: 1
	                                    },{
	                                        complete: function(){
	                                            if (index2 == 0) {
	                                            	// 放出最后一张后点击才有效
	                                                bSin = false;
	                                            }
	                                        }
	                                    });
	                                })(index);

	                                index--;
	                                if (index < 0) {
	                                    clearInterval(timer);
	                                }
	                            },200);
	                        }
	                    }
	                })
	            })(i);

	            i++;
	            if (i == aLi.length) {
	                clearInterval(timer);
	            }
	        },200);
	    };
	};
</script>
```
效果示例
![move框架应用 - 收起当前页放出下一页效果](http://ojvx9eehr.bkt.clouddn.com/move%E6%A1%86%E6%9E%B6%E5%BA%94%E7%94%A8%20-%20%E6%94%B6%E8%B5%B7%E5%BD%93%E5%89%8D%E9%A1%B5%E6%94%BE%E5%87%BA%E4%B8%8B%E4%B8%80%E9%A1%B5%E6%95%88%E6%9E%9C.gif)



## 12. 分块运动

1. 自定义行数R、列数C
2. 创建span
   计算oSpan的width、height、left、top、background-position

  > 注意：先appendChild才能获取oSpan的offsetWidth和offsetHeight

3. 分步运动，依次显示span

**优化：setInterval可用for循环+setTimeout替代，可设置行列相关时同一时间出现**

``` css
<style>
	* {
		margin: 0;
		padding: 0;
	}
	#btn{
		margin: 20px auto;
		padding: 5px;
		width: 100px;
		height: 20px;
		background: #f5850e;
		color: #fff;
		font-size: 16px;
		text-align: center;
		line-height: 20px;
		border-radius: 5px;
		cursor: pointer;
	}
	#box {
		position: relative;
		margin: 50px auto;
		width: 500px;
		height: 300px;
		background: url('img/slide0.jpg');
	}
	#box span{
		position: absolute;
		opacity: .1;
		/* opacity: 0; */
	}
</style>
```

``` html
	<div id='btn'>点击随机变换</div>
	<div id="box"></div>
```

``` javascript
<script type="text/javascript" src='move.js'></script>
<script>
	window.onload = function() {
		var oBtn = document.body.children[0];
		var oBox = document.body.children[1];

		var R = 3;
		var C = 5;

		// 整图 -> 分块(绝对定位)
		for(var r = 0; r < R; r++) {
			for(var c = 0; c < C; c++) {
				var oSpan = document.createElement('span');
				oSpan.style.width = oBox.offsetWidth / C + 'px';
				oSpan.style.height = oBox.offsetHeight / R + 'px';
				oBox.appendChild(oSpan);
				oSpan.style.left = oSpan.offsetWidth * c + 'px';
				oSpan.style.top = oSpan.offsetHeight * r + 'px';
				oSpan.style.backgroundPosition = - oSpan.offsetWidth * c + 'px ' + (-oSpan.offsetHeight * r) + 'px';
				oSpan.r = r;
				oSpan.c = c;
			}
		}

		var aSpan = oBox.children;
		var iNow = 0;
		var bSin = false;

		oBtn.onclick = function() {
			if(bSin) {return;}
			bSin = true;
			iNow++;
			oBox.style.background = 'url("img/slide'+ (iNow%9-1+9)%9 +'.jpg")';

			block(parseInt(Math.random()*7+1));		// 随机变换显示方式
		}

		var json = {};
		for(var i = 0; i < 8; i++) {
			json[i] = 0;
		}

		function block(method) {
			json[method]++;
			var med = 1;
			for(var i = 0; i < aSpan.length; i++) {
				(function(index) {
					switch(method) {
						case 1: med = index;
							break;
						case 2: med = aSpan[index].r + aSpan[index].c;  // 斜角显示
							break;
						case 3: med = aSpan[index].r * aSpan[index].c;
							break;
						case 4: med = aSpan[index].r - aSpan[index].c;
							break;
						case 5: med = aSpan[index].r / aSpan[index].c;
							break;
						case 6: med = aSpan.length - index;  // 由下至上
							break;
						case 7: med = Math.random();
							break;
					}
					setTimeout(function(){
						aSpan[index].style.backgroundImage = 'url("img/slide'+ iNow%9 + '.jpg")';
						aSpan[index].style.opacity = 0.1;
						(function(index2) {
							move(aSpan[index], {opacity: 1}, {complete: function() {
									if(index2 == aSpan.length - 1) {
										// 放出最后一个分块后点击才有效
										bSin = false;
									}
							}});
						})(index);
					}, 100*(med));	// 每个分块延迟时间不同，达到依次显示的效果
				})(i);
			}
		}
	}
</script>
```
效果示例
![move框架应用 - 分块运动](http://ojvx9eehr.bkt.clouddn.com/move%E6%A1%86%E6%9E%B6%E5%BA%94%E7%94%A8%20-%20%E5%88%86%E5%9D%97%E8%BF%90%E5%8A%A8.gif)


## 13. 仿Mac 感应变大效果

1. 感应距离：一般为500

  比例：scale = 1 - c/500;

2. 勾股定理计算鼠标至图片中心距离

  var a = getPos(aImg[i]).left + aImg[i].offsetWidth / 2 - oEvent.clientX;

  var b = getPos(aImg[i]).top + aImg[i].offsetHeight / 2 - oEvent.clientY;

 var c = Math.sqrt(a * a + b * b);

3. 计算方放大比例，范围为[0.5, 1]
  var scale = 1 - c / 500;
  scale < 0.5 && (scale = 0.5);
  aImg[i].style.width = scale * 80 + 'px';


``` css
<style>
	* {
		margin: 0;
		padding: 0;
	}

	#box {
		position: absolute;
		bottom: 20px;
		width: 100%;
		text-align: center;
	}
</style>
```

``` html
	<div id="box">
		<img src="img/per-1.png" width="40">
		<img src="img/per-2.png" width="40">
		<img src="img/per-3.png" width="40">
	</div>
```

``` javascript
<script>
	function getPos(obj) {
		var l = 0;
		var t = 0;
		while(obj) {
			l += obj.offsetLeft;
			t += obj.offsetTop;
			obj = obj.offsetParent;
		}

		return {left: l, top: t};
	}
	window.onload = function() {
		var oBox = document.body.children[0];
		var aImg = oBox.children;

		document.onmousemove = function(ev) {
			var oEvent = ev || event;
			for(var i = 0; i < aImg.length; i++) {

				// 勾股定理计算鼠标至图片中心距离
				var a = getPos(aImg[i]).left + aImg[i].offsetWidth / 2 - oEvent.clientX;
				var b = getPos(aImg[i]).top + aImg[i].offsetHeight / 2 - oEvent.clientY;
				var c = Math.sqrt(a * a + b * b);

				//计算方放大比例，范围为[0.5, 1]
				var scale = 1 - c / 500;
				scale < 0.5 && (scale = 0.5);
				aImg[i].style.width = scale * 80 + 'px';
			}
		}
	}
</script>
```
效果示例
![move框架应用 - 感应变大效果](http://ojvx9eehr.bkt.clouddn.com/move%E6%A1%86%E6%9E%B6%E5%BA%94%E7%94%A8%20-%20%E6%84%9F%E5%BA%94%E5%8F%98%E5%A4%A7.gif)


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)