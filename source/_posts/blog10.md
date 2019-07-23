---
title: 每天10个前端知识点：原生运动框架
date: 2017-02-01 03:27:35
categories: 前端札记
tags: [js]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。




---
*原生js编写运动框架，当然部分特性可用CSS3实现。*


---

## move.js

<!-- more -->

``` javascript
<script>

	function move(obj, json, options) {

		// 运动物体，{改变的属性及终态}，{总时间，运动形式，链式运动的回调}

		options = options || {};
		options.duration = options.duration || 700;  // 默认总时间 [可自定义]
		options.easing = options.easing || 'linear';  // 默认运动形式 [可自定义]

		clearInterval(obj.timer);

		var start = {};  // 起点
		var dis = {};  // 总距离

		for(var name in json) {
			start[name] = parseFloat(getStyle(obj, name));  // 字符串转数字，并保留小数(透明度)
			dis[name] = json[name] - start[name];
		}

		var count = Math.floor(options.duration / 30);  // 总次数 30ms 最佳定时器时间
		var n = 0;
		obj.timer = setInterval(function(){  // 自定义属性加定时器
			n++;
			for(var name in json) {
				switch(options.easing) {  // 自定义运动形式
					case 'linear':
						var a = n / count;
						var cur = start[name] + dis[name] * a;  // 匀速
						break;
					case 'ease-in':
						var a = n / count;
						var cur = start[name] + dis[name] * Math.pow(a, 3);  // 加速 a的3次方 [可自定义]
						break;
					case 'ease-out':
						var a = 1 - n / count;
						var cur = start[name] + dis[name] * (1 - Math.pow(a, 3));  // 减速 [可自定义]
						break;
				}

				if(name == 'opacity') {  // 若改变属性为透明度时另作处理
					obj.style.opacity = cur;
					obj.style.filter = 'Alpha(opacity:' + cur * 100 + ')';
				} else {
					obj.style[name] = cur + 'px';
				}
			}

			if( n == count) {
				clearInterval(obj.timer);
				options.complete && options.complete();  // 链式运动
			}
		}, 30);  // 30ms 最佳定时器时间
	}

	function getStyle(obj, name) {
		return (obj.currentStyle || getComputedStyle(obj, false))[name];
	}


</script>
```

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)