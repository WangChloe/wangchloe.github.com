---
title: 每天10个前端知识点：原生篇(2)
date: 2017-01-12 21:00:28
categories: 前端札记
tags: [js]

---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

{% aplayer "风向仪" "Depe" "http://ojvx9eehr.bkt.clouddn.com/Depe%20-%20%E9%A3%8E%E5%90%91%E4%BB%AA.mp3" %}


---
*本次内容主要关于js事件基础以及DOM操作元素。*

---
## 1. js操作元素属性
- **.** 属性操作符(不可以接收变量)
- **[]** 中括号可以操作属性也可以接收变量

``` javascript
<script>
	function setValue() {
		//省略获取元素oBtn, oBtn2的伪代码
		oBtn.value = 'bbb';
		oBtn2['value'] = 'bbb';

	}
</script>
```

>凡是**.** 出现的地方都可以用中括号替代



<!-- more -->

## 2. js设置复杂样式

> 非首单词的首字母大写并去掉-符

``` css
<style>
	.complex {
		margin-left: 10px;
	}
</style>
```
``` javascript
<script>
	function setStyle() {
		var oC = document.getElementsByClassName('complex')[0];
		oC.style.marginLeft = '10px';
	}
</script>
```

## 3. 物体从中心放大

> * margin-top: -变化的高度/2
> * margin-left: -变化的宽度/2

## 4. a链接防止刷新

``` html
<a href="#">点击会刷新页面</a>
```
``` javascipt
<a href="javascript:;">点击不会刷新页面！(推荐使用)</a>
<!-- javascript:;相当于一个伪协议 -->
```

## 5. 循环添加事件，事件中的循环变量不能用

``` javascript
<script>
	function clickEg() {
		//获取一组按钮	ps:js一组元素不能一起改变样式或设置事件
		var aBtn = document.getElementsByTagName('button');
		for(var i = 0; i < 3; i++) {
			aBtn[i].onclick = function() {
				//循环中的i变量此时已自增至3
				//aBtn[i].style.background = '#f00';

				//当前事件发生的对象 aBtn[i]为this
				this.style.background = 'f00';
			}
		}
	}
</script>

```
## 6. 浏览器加载的过程

1.加载整个页面的标签和属性
2.过滤不符合W3C标准的标签和属性(高级浏览器)
3.执行js -> window.onload

## 7. DOM获取元素方法
1.document.getElementById('id');
2.document/obj.getElementsByTagName('TagName');
3.document/obj.getElementsByClassName('ClassName');
兼容：Chrome、FF、IE9+

obj.getElementsByClassName
- 高级浏览器 -> function
- IE8- -> undefined
#### 兼容写法
``` javascript
<script>
	function getByClass(obj, sClass) {	// obj为从哪个父级下面查找类为sClass的元素
			if(obj.getElementsByClassName) {	// IE8- -> undefined  高级浏览器 -> function
				return obj.getElementsByClassName(sClass);	// 高级浏览器
			} else {	// IE8
				var aEle = document.getElementsByTagName('*');
				var arr = [];
				for(var i=0; i<aEle.length; i++){
					var temp = aEle[i].className.split(' ');
					if(findInArr(sClass, temp)) {
						arr.push(aEle[i]);
					}
				}
				return arr;
			}
		}
</script>
```

> * getElementById只能从document下获取
	var oDiv = document.getElementById('id');
> * getElementsByTagName/getElementsByClassName可以从document下获取，也可以从父级下获取
	var oDiv2 = document.getElementsByClassName('ClassName')[0];
	var oDiv3 = oBox.getElementsByTagName('TagName')[0];

## 8. js中的真假
- 真：非0数字，非空字符串，true，非空对象
- 假：0，空字符串('')，false，空对象(null)，undefined，NaN

## 9. 获取元素当前样式(兼容)
``` javascript
<script>
	function getStyle(obj, name){	//元素，样式名称
		if(obj.currentStyle) {	// Chrome、FF -> undefined	IE -> object
			// IE系
			return obj.currentStyle[name];	// 兼容IE系
		} else {
			// Chrome、FF
			return getComputedStyle(obj, false)[name];	// 兼容高级浏览器(Chrome、FF、IE9+)
		}

	}
</script>
```
简化
``` javascript
<script>
	function getStyle(obj, name){	//元素，样式名称
		return (obj.currentStyle || getComputedStyle(obj, false))[name];
	}

	// 调用
	console.log(parseInt(getStyle(oDiv, 'heihgt')));
</script>
```
## 10. 获取一个n~m之间的随机数(n<m，且不包括m)
``` javascript
<script>
	function rnd(n, m) {
		return parseInt(Math.random() * (m - n) + n);
	}
</script>
```

>应用：随机变色

``` javascript
<script>
	// rgb色值范围[0, 255]
	oDiv.style.background = 'rgb(' + rnd(0, 256) + ',' + rnd(0, 256) + ',' + rnd(0, 256) + ')';
</script>
```

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)