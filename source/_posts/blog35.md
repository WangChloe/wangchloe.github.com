---
title: 每天10个前端知识点：HTML5(canvas应用)
date: 2017-04-23 03:27:35
categories: 前端札记
tags: [HTML5]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---


- \(1\) 应用 canvas笑脸
- \(2\) 应用 canvas画图
- \(3\) 应用 canvas变换
- \(4\) 应用 canvas内长方形拖拽
- \(5\) 应用 canvas内圆形拖拽
- \(6\) 应用 下载canvas绘图
- \(7\) 应用 canvas运动回调
- \(8\) canvas框架 jCanvaScript.js

<!-- more -->

---

### (1) 应用 canvas笑脸
```
<canvas width="800" height="1200"></canvas>
<script>
	var oC = document.querySelector('canvas');
	var ctx = oC.getContext('2d');

	// face
	ctx.fillStyle = 'yellow';
	ctx.arc(400, 250, 180, 0, 2 * Math.PI, false);
	ctx.fill();
	ctx.stroke();

	// mouth
	ctx.beginPath();
	ctx.lineCap = 'round';

	ctx.lineWidth = 20;
	ctx.fillStyle = 'red';
	ctx.arc(400, 280, 80, 0, Math.PI, false);
	// ctx.fill();  // 红色实心半圆张嘴笑脸
	ctx.stroke();  // 黑色半圆弧微笑脸

	// eyes
	ctx.beginPath();
	ctx.fillStyle = 'black';
	ctx.moveTo(350, 200);
	ctx.arc(350, 200, 20, 0, 2 * Math.PI, false);

	ctx.moveTo(450, 200);
	ctx.arc(450, 200, 20, 0, 2 * Math.PI, false);

	ctx.closePath();

	ctx.fill();
</script>
```

### (2) 应用 canvas画图
```
<canvas width="800" height="600"></canvas>
<script>
	var oC = document.querySelector('canvas');
	var ctx = oC.getContext('2d');

	oC.onmousedown = function(ev) {

		var {
			clientX,
			clientY
		} = ev;

		ctx.moveTo(clientX, clientY)

		oC.onmousemove = function(ev) {

			ctx.clearRect(0, 0, oC.width, oC.height);

			var {
				clientX,
				clientY
			} = ev;

			ctx.lineWidth = 5;
			ctx.lineTo(clientX, clientY);
			ctx.stroke();
		}

		oC.onmouseup = function() {
			oC.onmousemove = null;
			oC.onmouseup = null;
		}
	}
</script>
```

### (3) 应用 canvas变换
```
<canvas width="800" height="600"></canvas>
	<script>
		var oC = document.querySelector('canvas');
		var ctx = oC.getContext('2d');

		var degree = 1;

		var x1 = 300;
		var y1 = 300;
		var w1 = 100;
		var h1 = 100;

		var x2 = 100;
		var y2 = 100;
		var w2 = 50;
		var h2 = 50;

		var rotate1 = 0;
		var rotate2 = 0;

		setInterval(function() {
			ctx.clearRect(-oC.width, -oC.height, oC.width * 2, 2 * oC.height);

			ctx.save();
			ctx.translate(x1, y1);
			ctx.rotate(rotate1);
			ctx.fillRect(-w1 / 2, -h1 / 2, w1, h1);
			ctx.restore();

			ctx.save();
			ctx.translate(x2, y2);
			ctx.rotate(rotate2);
			ctx.fillRect(-w2 / 2, -h2 / 2, w2, h2);
			ctx.restore()

			rotate1++;
			rotate2 += 0.2;
		}, 100)

		function d2a(deg) {
			return deg / 180 * Math.PI
		}
	</script>
```

### (4) 应用 canvas内长方形拖拽
```
<canvas width="800" height="600"></canvas>
<script>
	var oC = document.querySelector('canvas');
	var ctx = oC.getContext('2d');

	var rect = {
		x: 0,
		y: 0,
		w: 100,
		h: 100
	}

	ctx.fillRect(
		rect.x,
		rect.y,
		rect.w,
		rect.h
	);

	oC.onmousedown = function({
		clientX,
		clientY
	}) {
		var {
			x,
			y,
			w,
			h
		} = rect;

		var disX = clientX - x;
		var disY = clientY - y;

		if (
			clientX > x &&
			clientX < x + w &&
			clientY > y &&
			clientY < y + h
		) {
			oC.onmousemove = function({
				clientX,
				clientY
			}) {

				ctx.clearRect(0, 0, oC.width, oC.height)  // **先清空画布再画

				var deltaX = clientX - disX;
				var deltaY = clientY - disY;

				rect.x = deltaX;
				rect.y = deltaY;

				ctx.fillRect(
					rect.x,
					rect.y,
					rect.w,
					rect.h
				);
			}

			oC.onmouseup = function() {
				oC.onmousemove = null;
				oC.onmouseup = null;
			}
		}
	}
</script>
```

### (5) 应用 canvas内圆形拖拽
```
<canvas width="800" height="600"></canvas>
<script>
	var oC = document.querySelector('canvas');
	var ctx = oC.getContext('2d');

	var circle = {
		x: 50,
		y: 50,
		r: 50
	}

	ctx.arc(circle.x, circle.y, circle.r, 0, 2 * Math.PI, false);

	ctx.fill();

	oC.onmousedown = function({
		clientX,
		clientY
	}) {
		var {
			x,
			y,
			r
		} = circle;

		var disX = clientX - x;
		var disY = clientY - y;

		if (
			ctx.isPointInPath(clientX, clientY)  // 利用isPointInPath函数
		) {
			oC.onmousemove = function({
				clientX,
				clientY
			}) {
				ctx.clearRect(0, 0, oC.width, oC.height);
				ctx.beginPath();

				var deltaX = clientX - disX;
				var deltaY = clientY - disY;

				circle.x = deltaX;
				circle.y = deltaY;

				ctx.arc(circle.x, circle.y, circle.r, 0, 2 * Math.PI, false);
				ctx.fill();
			}

			oC.onmouseup = function() {
				oC.onmousemove = null;
				oC.onmouseup = null;
			}
		}
	}
</script>

```

### (6) 应用 下载canvas绘图
```
<canvas width="600" height="400"></canvas>
<br>
<button>Download</button>
<script>
	var oC = document.querySelector('canvas');
	var ctx = oC.getContext('2d');
	var oBtn = document.querySelector('button');

	var data = [
		rnd(100, 1000),
		rnd(100, 1000),
		rnd(100, 1000),
		rnd(100, 1000),
		rnd(100, 1000)
	]

	var start = 0;

	var sum = sumUp(data);

	data.forEach(function(number, index) {
		var color = `rgb(${rnd(0,255)},${rnd(0,255)},${rnd(0,255)})`;

		var delta = number / sum * 2 * Math.PI;

		ctx.fillStyle = color;

		ctx.beginPath();

		ctx.moveTo(300, 200)
		ctx.arc(300, 200, 100, start, start + delta, false);
		ctx.lineTo(300, 200)

		ctx.fill();

		start = start + delta;
	})

	function sumUp(array) {
		var sum = 0;

		array.forEach(function(n) {
			sum += n
		})

		return sum;
	}

	function rnd(n, m) {
		return parseInt(Math.random() * (m - n) + n);
	}

	// **下载canvas图片
	oBtn.onclick = function() {
		var oA = document.createElement('a');
		oA.href = oC.toDataURL();
		oA.download = '默认命名';
		// oA.download = fileName.value ? fileName.value : '默认命名' + '.png';

		oA.click();
	}
</script>
```

### (7) 应用 canvas运动回调
```
<script>
	function loadStatics(statics, callback) {
		var count = 0;

		statics.forEach(function(path, index) {
			var oImage = new Image();
			oImage.src = `img/${path}.png`

			resources[path] = oImage;

			oImage.onload = function() {

				count++

				if (count == statics.length) {
					callback && callback();
				}
			}
		})
	}

	function d2a(d) {
		return d / 180 * Math.PI
	}

	function a2d(a) {
		return a / Math.PI * 180
	}

	function rnd(n, m) {
		return parseInt(Math.random() * (m - n) + n)
	}

	function rndSign() {
		return Math.random() < 0.5 ? -1 : 1
	}
</script>
```

### (8) canvas框架 jCanvaScript.js
[jCanvaScript.js](http://jcscript.com/)

示例
```
<canvas id="c1" width="500" height="500"></canvas>
<script src="libs/jCanvaScript.1.5.18.min.js"></script>
<script>
    var idCanvas = "c1";
    onload_1();

    var interval_1 = 0;

    function startShow() {
        var r = Math.floor(Math.random() * (254)),
            g = Math.floor(Math.random() * (254)),
            b = Math.floor(Math.random() * (254)),
            x = Math.floor(Math.random() * (439)),
            y = Math.floor(Math.random() * (554)),
            color = "rgba(" + r + ", " + g + ", " + b + ", 0.5)",
            filled = true,
            radius = 1;
        jc.circle(x, y, radius, color, filled)
            .animate({
                radius: 100,
                opacity: 0
            }, 1500, function() {
                this.del();
            });
    }

    function onload_1() {
        jc.start(idCanvas, true);
        interval_1 = setInterval(startShow, 200);
    }

    function start_1(idCanvas) {
        if (interval_1) return;
        onload_1();
    }

    function stop_1(idCanvas) {
        clearInterval(interval_1);
        interval_1 = 0;
        jc.clear(idCanvas);
    }
</script>
```




---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！

公众号是坚持日更的，不发文的时候推送一些我觉得好用的前端网站或者看到的一些问题的解决方案，也更便于大家交流，就酱。

![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)