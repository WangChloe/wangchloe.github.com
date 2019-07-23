---
title: 每天10个前端知识点：HTML5(选择器、自定义属性、存储)
date: 2017-03-20 03:27:35
categories: 前端札记
tags: [HTML5]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。



---

- H5地理位置 geolocation
	- \(1\) 测试用例
	- \(2\) 百度地图API
- H5音频 audio
	- \(1\) 属性
	- \(2\) 方法
	- \(3\) 应用：钢琴弹奏
	- \(4\) 应用：音乐播放器

<!-- more -->

---


## 7. H5地理位置 geolocation

LBS -> Location Based Service 基于位置服务

### (1) 测试用例

``` javascript
<script>
	window.navigator.geolocation.getCurrentPosition(function(res) {
		console.log(res);
	}, function(err) {
		console.log(err);
	})
</script>
```

#### PositionError
错误码 错误信息

- `code: 1`
`message: "User denied Geolocation"` -> 用户拒绝授权

- `code: 1`
`message: "Only secure origins are allowed (see: https://goo.gl/Y0ZkNV).` -> 仅允许HTTPS访问

- `code:2`
`message:"Network location provider at 'https://www.googleapis.com/' : No response received."` -> 没翻墙

### (2) 百度地图API
- timestamp 时间戳

- coords -> 地理坐标
  - accuracy:26 -> 精确度
  - altitude:null -> 海拔
  - altitudeAccuracy:null -> 海拔高度精确度
  - heading:null -> 方向
  - latitude:31.167638 -> 纬度
  - longitude:121.423593 -> 经度
  - speed:null -> 速度

```
<!DOCTYPE html>
<html>
<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<meta name="viewport" content="initial-scale=1.0, user-scalable=no" />
	<style type="text/css">
		body, html,#allmap {
			user-select: none;
			width: 100%;
			height: 100%;
			overflow: hidden;
			margin: 0;
			font-family: "微软雅黑";
		}
	</style>
	<script type="text/javascript" src="http://api.map.baidu.com/api?v=2.0&ak=wmwHFMPxi66GlPBVUrdgEhDzbLUqlSrM"></script>
	<title>上海师范大学 - 百度地图</title>
</head>
<body>
	<div id="allmap"></div>
</body>
</html>
<script type="text/javascript">
	var map = new BMap.Map("allmap");

	map.centerAndZoom(new BMap.Point(121.423593, 31.167638), 20); // 初始化地图,设置中心点坐标和地图级别

	map.addControl(new BMap.MapTypeControl()); //添加地图类型控件
	map.setCurrentCity("上海"); // 设置地图显示的城市 此项是必须设置的
	map.enableScrollWheelZoom(true); //开启鼠标滚轮缩放

	navigator.geolocation.getCurrentPosition(function(res) {
		var {
			coords: {
				longitude,
				latitude
			}
		} = res;

		var point = new BMap.Point(longitude, latitude);

		var marker = new BMap.Marker(point);
		map.addOverlay(marker);

		marker.setAnimation(BMAP_ANIMATION_BOUNCE)

		map.panTo(point)
	}, function(err) {
		console.log(err);
	})
</script>
```

## 8. H5音频 audio

- 音频格式：mp3 wma flat ape wav ogg

### (1) 属性
- src -> 音频路径
- controls -> 显示自带控制进度条
- loop -> 音频循环
- autoplay -> 自动播放 只有pc端可以实现
- muted -> 静音

- currentTime -> 当前播放时间
- duration -> 音频总时间
- volume -> 音量 [0,1]

- ontimeupdate -> 进度更新

- play -> 是否在播放 返回true/false
- pause -> 是否暂停 返回true/false

### (2) 方法
- play() -> 播放歌曲
- pause() -> 暂停歌曲
- load() -> 重新加载歌曲
- onended() -> 音频播放完毕

常见结构
``` html
<html>
	<audio src="xxx.mp3" controls></audio>  <!-- 显示音频自带播放器样式 -->
	<hr>
	<!-- 自定义播放器 -->
	<audio src="xxx.mp3" id="a1"></audio>  <!-- 只显示下面的按钮 -->
	<!-- 进度条 -->
	<div class="progress">
		<div class="inner"></div>
	</div>
	<input type="range" min="0" max="100" value="100">音量
	<button onclick="aPlay()">播放/暂停</button>
	<button onclick="aMute()">静音</button>
</html>
```

``` javascript
<script>
	var a1 = document.getElementById('a1');
	var progress = document.querySelector('.progress');
	var oInner = document.querySelector('.inner');
	var oRange = document.querySelector('[type=range]');

	// 自定义进度条
	// setInterval(function() {
	// 	oInner.style.width = (a1.currentTime / a1.duration) * 100 + '%';
	// }, 16);

	a1.ontimeupdate = function(){
		oInner.style.width = (a1.currentTime / a1.duration) * 100 + '%';
	}

	progress.onclick = function({
		clientX
	}) {
		var leftDelta = clientX - this.offsetLeft;

		var percentage = leftDelta / this.offsetWidth;

		a1.currentTime = a1.duration * percentage;
	}

	oRange.oninput = function(){
		a1.volume = this.value/100;
	}

	function aPlay() {
		if (a1.paused) {
			a1.play();
		} else {
			a1.pause();
		}
	}

	function aMute() {
		a1.muted = !oA.muted;
	}

</script>
```

### (3) 应用：钢琴弹奏

- **sound.js**

钢琴示例

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>钢琴示例</title>
	<style>
		body {
			user-select: none;
		}
		ul, li {
			list-style: none;
			margin: 0;
			padding: 0;
		}
		ul {
			width: 400px;
			margin: 40px auto;
		}
		li {
			width: 38px;
			border: 1px solid black;
			height: 198px;
			float: left;
			text-align: center;
			line-height: 350px;
			margin-left: 10px;
			transform-origin: top center;
		}
		li:active {
			transform: perspective(800px) rotateX(-10deg);
		}
		li.active {
			transform: perspective(800px) rotateX(-10deg);
		}
	</style>
</head>
<body>
	<ul>
		<li>1</li>
		<li>2</li>
		<li>3</li>
		<li>4</li>
		<li>5</li>
		<li>6</li>
		<li>7</li>
		<li>8</li>
	</ul>
	<script src="statics/sound.js"></script>
	<script>
		var aLi = document.querySelectorAll('li');

		aLi.forEach(function(oLi, index) {
			oLi.onmousedown = function() {
				playSound(index + 49);
			}
		})

		window.onkeydown = function({
			keyCode
		}) {
			playSound(keyCode);

			aLi[keyCode - 49].classList.add('active');
		}

		window.onkeyup = function({
			keyCode
		}) {
			if (keyCode >= 49 && keyCode <= 56) {
				aLi[keyCode - 49].classList.remove('active');
			}
		}

		function playSound(index) {
			new Audio(oggSound[`sound${index}`]).play();
		}
	</script>
</body>
</html>
```

### (4) 应用：音乐播放器

- 歌词显示

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		#box {
			width: 200px;
			margin: 40px auto;
			text-align: center;
			padding: 10px;
			border: 2px solid black;
			-webkit-background-clip: text;
			background-image: linear-gradient(90deg, red 30%, blue 30%);
			color: transparent;
		}
	</style>
</head>
<body>
	<div id="box">
		西湖的水我的泪
	</div>
	<script>
		var progress = 0;
		var oBox = document.querySelector('div');

		setInterval(function() {
			oBox.style.backgroundImage = `linear-gradient(90deg,red ${progress}%,blue ${progress}%)`

			progress += 0.4;
		}, 16)
	</script>
</body>
</html>
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)