---
title: 每天10个前端知识点：HTML5(内联SVG)
date: 2017-04-24 03:27:35
categories: 前端札记
tags: [HTML5]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

- H5内联SVG
	- \(1\) 位图 && 矢量图
	- \(2\) SVG使用
	- \(3\) SVG梗概
	- \(4\) SVG应用

<!-- more -->

---

## 14. H5内联SVG
SVG(Scalable Vector Graphic 可伸缩矢量图形)
VML(The Vector Markup Language  矢量可标记语言)

> SVG 使用 XML 格式定义图形

### (1) 位图 && 矢量图
- 位图 -> 由像素点构成的图形
  - 优点: 色彩信息相当复杂
  - 缺点: 失真、体积大

- 矢量图 -> 由数学语言描述出的图形
  - 优点: 体积小不失真
  - 缺点: 色彩信息单一，图形简单

### (2) SVG使用
1.
```
<img src="xxx.svg">
```
```
<?xml version="1.0" encoding="UTF-8"?>
<svg width="801px" height="792px" viewBox="0 0 801 792" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
	...
</svg>
```

2.
```
<svg>
	<rect x="20" y="20" rx="20" ry="20" width="250" height="250" style="fill:blue;stroke:pink;stroke-width:5;fill-opacity:0.1;stroke-opacity:0.9"/>
</svg>
```

3.
```
<svg width="200" height="200"></svg>
<script>
	var oSvg = document.querySelector('svg');
	var oLine = document.createElementNS('http://www.w3.org/2000/svg', 'line');

	oLine.setAttribute('x1', '10');
	oLine.setAttribute('y1', '10');
	oLine.setAttribute('x2', '100');
	oLine.setAttribute('y2', '100');
	oLine.setAttribute('stroke', 'green');

	oSvg.appendChild(oLine);
</script>
```


### (3) SVG梗概
- `<rect>`     矩形
  eg: `<rect x="20" y="20" rx="20" ry="20" width="250" height="250" style="fill:blue;stroke:pink;stroke-width:5;fill-opacity:0.1;stroke-opacity:0.9"/>`
  - x, y  位置
  - width, height  宽高
  - rx, ry  圆角矩形
  - style  css属性

- `<circle>`   圆形
  eg: `<circle cx="100" cy="50" r="40" stroke="black" stroke-width="2" fill="red"/>`
  - cx, cy  圆心坐标 默认(0, 0)
  - r  半径

- `<ellipse>`  椭圆
  eg: `<ellipse cx="300" cy="150" rx="200" ry="80"style="fill:rgb(200,100,50);stroke:rgb(0,0,100);stroke-width:2"/>`
  - cx, cy  圆心坐标
  - rx, ry  水平/垂直半径

- `<line>`     线
  eg: `<line x1="0" y1="0" x2="300" y2="300"style="stroke:rgb(99,99,99);stroke-width:2"/>`
  - x1, y1  开始坐标
  - x2, y2  结束坐标

- `<polyline>` 折线
  eg: `<polyline points="0,0 0,20 20,20 20,40 40,40 40,60"style="fill:white;stroke:red;stroke-width:2"/>`
  - points 每个折点的x,y坐标

- `<polygon>`  多边形
  eg: `<polygon points="220,100 300,210 170,250"style="fill:#cccccc;stroke:#000000;stroke-width:1"/>`
  - points  每个角的x,y坐标(自动闭合)

- `<path>`     路径
  eg: `<path d="M250 150 L150 350 L350 350 Z" style="#ccc;"/>`  **大写表示绝对定位，小写表示相对定位。**
  - M  moveto
  - L  lineto
  - H  horizontal lineto
  - V  vertical lineto
  - C  curveto
  - S  smooth curveto
  - Q  quadratic Belzier curve
  - T  smooth quadratic Belzier curveto
  - A  elliptical Arc
  - Z  closepath


- `<filter>`    高斯滤镜  **<filter> 标签必须嵌套在 <defs> 标签内。**  definitions 定义
  eg: `<defs><filter id="Gaussian_Blur"><feGaussianBlur in="SourceGraphic" stdDeviation="3" /></filter></defs>`
  `<ellipse cx="200" cy="150" rx="70" ry="40" style="fill:#ff0000;stroke:#000000;stroke-width:2;filter:url(#Gaussian_Blur)"/>`
  - <filter>id  定义名称

  - filter:url(#xxx)  链接滤镜

- `<linearGradient>`  线性渐变  **<linearGradient> 标签必须嵌套在 <defs> 的内部。**
  eg: `<defs><linearGradient id="orange_red" x1="0%" y1="0%" x2="100%" y2="0%"><stop offset="0%" style="stop-color:rgb(255,255,0);stop-opacity:1"/><stop offset="100%" style="stop-color:rgb(255,0,0);stop-opacity:1"/></linearGradient></defs>`
  `<ellipse cx="200" cy="190" rx="85" ry="55" style="fill:url(#orange_red)"/>`
  - <linearGradient>id  定义名称
  - <linearGradient>x1,y1  渐变开始位置
  - <linearGradient>x2,y2  渐变结束位置
  - <stop>  渐变颜色

  - fill:url(#xxx)  链接渐变

- `<radialGradient>`  径向渐变  **<radialGradient> 标签必须嵌套在 <defs> 中。**
  eg: `<defs><radialGradient id="grey_blue" cx="50%" cy="50%" r="50%"fx="50%" fy="50%"><stop offset="0%" style="stop-color:rgb(200,200,200);stop-opacity:0"/><stop offset="100%" style="stop-color:rgb(0,0,255);stop-opacity:1"/></radialGradient></defs>`
  `<ellipse cx="230" cy="200" rx="110" ry="100" style="fill:url(#grey_blue)"/>`
  - <radialGradient>id  定义名称
  - <radialGradient>cx,cy,r  外圈
  - <radialGradient>fx,fy  内圈
  - <stop>  渐变颜色

  - fill:url(#xxx)  链接渐变

### (4) SVG应用

[纯CSS实现帅气的SVG路径描边动画效果](http://www.zhangxinxu.com/wordpress/2014/04/animateion-line-drawing-svg-path-%E5%8A%A8%E7%94%BB-%E8%B7%AF%E5%BE%84/)

- `stroke-dasharray`  各虚线长度
- `stroke-dashoffset`  虚线的起始偏移

获取路径长度
```
<script>
	var path = document.querySelector('path');
	var length = path.getTotalLength();
</script>
```

#### 2017

``` css
<style>
	.one {
		stroke-dasharray: 600;
		stroke-dashoffset: 600;
		animation: ani 2s forwards;
	}
	.two {
		stroke-dasharray: 600;
		stroke-dashoffset: 600;
		animation: ani 1.8s forwards 0.2s;
	}
	.three {
		stroke-dasharray: 600;
		stroke-dashoffset: 600;
		animation: ani 1.6s forwards 0.4s;
	}
	@keyframes ani {
		to {
			stroke-dashoffset: 0;
		}
	}
</style>
```

三个颜色变换

``` html
<svg width="800" height="600">
	<!-- one -->
	<path d="M149.593084,114.512754 C149.593084,80.6086232 265.458217,31.6263568 265.458217,144.647923 C265.458217,257.669489 149.593084,288.726563 149.593084,288.726563 L291.550902,288.726563" class="one" stroke="#4A4A4A" stroke-width="8" fill="none" stroke-lineCap="round"></path>
	<path d="M391.00506,81.6608912 C297.400634,81.6608912 275.086722,284.767558 384.426619,284.767558 C493.766517,284.767558 484.609487,81.6608912 391.00506,81.6608912 Z" class="one"  stroke="#4A4A4A" stroke-width="8" fill="none" stroke-lineCap="round"></path>
	<path d="M508.720275,85.6448695 C519.572766,106.125165 510.370277,296.381449 510.370277,296.381449" class="one"  stroke="#4A4A4A" stroke-width="8" fill="none" stroke-lineCap="round"></path>
	<path d="M570.187369,93.0436865 C570.187369,93.0436865 703.312123,69.0165013 703.312123,103.354018 C703.312123,137.691534 595.118196,304.797696 595.118196,304.797696" class="one"  stroke="#4A4A4A" stroke-width="8" fill="none" stroke-lineCap="round"></path>
	<!-- two -->
	<path d="M149.593084,114.512754 C149.593084,80.6086232 265.458217,31.6263568 265.458217,144.647923 C265.458217,257.669489 149.593084,288.726563 149.593084,288.726563 L291.550902,288.726563" class="two" stroke="#029df9" stroke-width="8" fill="none" stroke-lineCap="round"></path>
	<path d="M391.00506,81.6608912 C297.400634,81.6608912 275.086722,284.767558 384.426619,284.767558 C493.766517,284.767558 484.609487,81.6608912 391.00506,81.6608912 Z" class="two" stroke="#029df9" stroke-width="8" fill="none" stroke-lineCap="round"></path>
	<path d="M508.720275,85.6448695 C519.572766,106.125165 510.370277,296.381449 510.370277,296.381449" class="two" stroke="#029df9" stroke-width="8" fill="none" stroke-lineCap="round"></path>
	<path d="M570.187369,93.0436865 C570.187369,93.0436865 703.312123,69.0165013 703.312123,103.354018 C703.312123,137.691534 595.118196,304.797696 595.118196,304.797696" class="two" stroke="#029df9" stroke-width="8" fill="none" stroke-lineCap="round"></path>
	<!-- three -->
	<path d="M149.593084,114.512754 C149.593084,80.6086232 265.458217,31.6263568 265.458217,144.647923 C265.458217,257.669489 149.593084,288.726563 149.593084,288.726563 L291.550902,288.726563" class="three" stroke="#90da32" stroke-width="8" fill="none" stroke-lineCap="round"></path>
	<path d="M391.00506,81.6608912 C297.400634,81.6608912 275.086722,284.767558 384.426619,284.767558 C493.766517,284.767558 484.609487,81.6608912 391.00506,81.6608912 Z" class="three" stroke="#90da32" stroke-width="8" fill="none" stroke-lineCap="round"></path>
	<path d="M508.720275,85.6448695 C519.572766,106.125165 510.370277,296.381449 510.370277,296.381449" class="three" stroke="#90da32" stroke-width="8" fill="none" stroke-lineCap="round"></path>
	<path d="M570.187369,93.0436865 C570.187369,93.0436865 703.312123,69.0165013 703.312123,103.354018 C703.312123,137.691534 595.118196,304.797696 595.118196,304.797696" class="three" stroke="#90da32" stroke-width="8" fill="none" stroke-lineCap="round"></path>
</svg>
```

### (5) 矢量图形库 Raphael.js
[Raphaël Reference](http://dmitrybaranovskiy.github.io/raphael/reference.html)

示例
```
<script src="libs/raphael.min.js"></script>
<script>
	// Creates canvas 320 × 200 at 10, 50
	var paper = Raphael(10, 50, 320, 200);

	// Creates circle at x = 50, y = 40, with radius 10
	var circle = paper.circle(50, 40, 10);
	// Sets the fill attribute of the circle to red (#f00)
	circle.attr("fill", "#f00");

	// Sets the stroke attribute of the circle to white
	circle.attr("stroke", "#fff");

	circle.click(function() {
		this.animate({
			fill: '#fe0',
			y: 100
		}, 500, 'bounce');
	})

	// circle.drag(function(dx,dy){
	// 	this.attr({
	// 		x: x + dx,
	// 		y: y + dy
	// 	})
	// },function(){
	// 	x = this.attr('x')
	// 	y = this.attr('y')
	// })
</script>
```



---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！

公众号是坚持日更的，不发文的时候推送一些我觉得好用的前端网站或者看到的一些问题的解决方案，也更便于大家交流，就酱。

![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)