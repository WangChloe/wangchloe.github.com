---
title: 每天10个前端知识点：事件篇
date: 2017-01-23 03:27:35
categories: 前端札记
tags: [js]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。




---
*将事件的冒泡、绑定、捕获、委托及鼠标和键盘的相关事件整理了下。*


---

## 1. 事件对象
- event  事件对象(系统自带)  
 *兼容：Chrome IE系
  FF -> 报错
- ev     事件函数传入参数 
  *兼容：高级浏览器(Chrome、FF、IE9+)
   IE8- -> undefined

**兼容写法**：var oEvent = ev || event;

<!-- more -->

## 2. 事件冒泡
概念：子级的事件会传递给父级。如果父级有相同的事件，会依次从内到外执行，直到相同事件的祖宗节点，否则会继续冒泡。

**阻止事件冒泡：**

子级事件内添加  `oEvent.cancelBubble = true;`

## 3. 事件绑定
**有效解决事件冲突**

- obj.addEventListener(事件名, 函数名/函数, 是否捕获);

*兼容：高级浏览器

事件名 -> 不能加'on'

函数名 -> 不能加括号

是否捕获 -> false

- obj.attachEvent(事件名, 函数名/函数);

*兼容：IE10-

事件名 -> 必须加'on'

函数名 -> 不能加括号

**兼容写法**

封装一个事件绑定的函数
``` javascript
<script>
	function addEvent(obj, sEv, fn) {	//对象, 事件(不加on), 函数名/函数
		if(obj.addEventListner) {	//高级浏览器 -> function  低级 ->undefined
			//高级浏览器
			obj.addEventListener(sEv, fn, false);
		} else {
			//低级浏览器
			obj.attachEvent('on' + sEv, fn);
		}
	}
</script>
```


## 4. 关于捕获(这点理解不是很到位)
事件冒泡：子级 -> 父级
设置捕获：父级 -> 子级

## 5. 事件解绑
- obj.removeEventListener(事件名, 函数名/函数, 是否捕获);

*兼容：高级浏览器

**注意：函数不能是匿名函数，每个匿名函数都相当于新创建了一个函数。**

创建函数 var show = new Function('a','b', 'alert(a + b)');

- obj.detachEvent(事件名, 函数名/函数);

*兼容：IE10-

**兼容写法**

封装一个事件解绑的函数
``` javascript
<script>
	function removeEvent(obj, sEv, fn) {	//对象, 事件(不加on), 函数名/函数
		if(obj.removeEventListner) {	//高级浏览器 -> function  低级 ->undefined
			//高级浏览器
			obj.removeEventListener(sEv, fn, false);
		} else {
			//低级
			obj.detachEvent('on' + sEv, fn);
		}
	}
</script>
```


## 6. 查看鼠标点击位置

var oEvent = ev || event;

X轴：oEvent.clientX;

Y轴：oEvent.clientY;

## 7. 键盘事件

1. obj.onkeydown  按下键盘触发
2. obj.onkeyup    释放键盘触发
3. obj.oninput	 	  键盘输入时实时触发
*兼容：高级浏览器
IE9删除时有问题

- obj.onpropertychange	键盘输入时实时触发
*兼容：IE10-
IE9删除时有问题


**兼容处理**

事件的兼容不需要处理，直接连等

obj.oninput = obj.onpropertychange = function() {}

*处理IE9：定时器


封装一个实时统计字数的函数
``` javascript
<script>
	function calLen(obj1, obj2) {
		if (window.navigator.userAgent.indexOf('MSIE 9.0') != -1) { //IE9
			var timer = null;
			obj1.onfocus = function() {
				timer = setInterval(function() {
					obj2.innerHTML = obj1.value.length;
				}, 50);
			};
			obj1.onblur = function() {
				clearInterval(timer);
			}
		} else { //能不添加定时器时就不添加
			obj1.oninput = obj1.onpropertychange = function() { //高级浏览器、IE10-
				obj2.innerHTML = obj1.value.length;
			}
		}
	}
</script>
```

应用：实时统计输入字数

### 键码 `oEvent.keyCode`

**重要**
- 0~9：48~57
- a~z：65~90
- ctrl：17
- delete：46
- backspace：8
- enter：13
- 左键：37
- 上键：38
- 右键：39
- 下键：40

> 组合键(js中键码不能组合使用)

- ctrl  -> ctrlKey
- shift -> shiftKey
- alt   -> altKey

eg:
`if(oEvent.ctrlKey && oEvent.shiftKey && oEvent.keyCode == 65){...}`

## 8. 鼠标事件
1. obj.oncontextmenu 点击鼠标右键触发(有默认右键菜单行为)

  > 默认行为：
  > 点击右键有菜单 文本框能输入内容 点击a标签能跳转等

  **阻止默认行为：return  false;**

  应用：自定义右键菜单
  应用：自定义输入框

2. obj.onmousedown	按下鼠标时触发
3. obj.onmouseup	抬起鼠标时触发
4. obj.onmousemove	鼠标移动触发

  应用：拖拽
  应用：拖拽(带框)
  应用：磁性吸附

5. obj.onmouseover  鼠标移入时触发

6. obj.onmouseout   鼠标移出时触发

  **问题**

  问题1：移入子级也算重新移入
  解决1：onmouseover -> onmouseenter

  问题2：移出子级也算移出
  解决2：onmouseout -> onmouseleave

7. obj.onmousewheel  滚动鼠标滚轮触发

*兼容：Chrome IE系

DOMMouseScroll       DOM滚轮事件(**DOM事件只能通过事件绑定添加**)

*兼容：FF

**兼容写法**
``` javascript
<script>
	if (window.navigator.userAgent.indexOf('FireFox') != -1) {
		document.addEventListener('DOMMouseScroll', function() { //FF
			//scroll code here
		}, false)
	} else {
		document.onmousewheel = function() { //Chrome IE系
			//scroll code here
		}
	}
</script>
```

### 判断滚动方向
- oEvent.wheelDelta

	*兼容：Chrome IE系

	向上：120

	向下：-120

- DOMMouseScroll

	*兼容：FF

	向上：-3

	向下：3

**兼容写法**

封装一个鼠标滚动方向的函数

``` javascript
<script>
	function addWheel(obj, fn) { //向上fn(false)，向下fn(true)
		function wheel(ev) {
			var oEvent = ev || event;

			// var bDown = true;				//默认向下 -->
			// if(oEvent.wheelDelta) {			//FF -> undefined
			// 	//Chrome IE系
			// 	bDown = oEvent.wheelDelta < 0;
			// } else {
			// 	//FF
			// 	bDown = oEvent.detail > 0;
			// }

			var bDown = oEvent.wheelDelta ? oEvent.wheelDelta < 0 : oEvent.detail > 0;

			//判断是否传入函数，执行回调函数
			fn && fn(bDown);

			//FF阻止默认
			oEvent.preventDefault && oEvent.preventDefault();

			//阻止默认
			return false;
		}

		if (window.navigator.userAgent.indexOf('FireFox') != -1) {
			//FF
			document.addEventListener('DOMMouseScroll', wheel, false); //事件中阻止默认没有用
		} else {
			//Chrome IE系
			// document.onmousewheel = wheel;
			addEvent(obj, 'mousewheel', wheel);
		}
	}
</script>

```

  > oEvent.preventDefault();
  兼容：高级浏览器
  IE8- -> undefined

  应用：自定义滚动条

## 9. domReady
- DOMContentLoaded 当DOM加载完成时触发(在页面前) **DOM事件必须通过事件绑定添加**
*兼容：高级浏览器

- onreadystatechange 模拟domReady

**兼容写法**
封装domReady全兼容方法

``` javascript
<script>
	function domReady(fn) {
		if (document.addEventListener) {
			//高级浏览器
			document.addEventListener('DOMContentLoaded', function() {
				fn && fn();
			}, false);
		} else {
			//低级浏览器  模拟domReady
			document.onreadystatechange = function() {
				if (document.readyState == 'complete') {	// 全兼容
					fn && fn();
				}
			}
		}
	}
</script>
```

## 10. 事件委托
概念：子级自己的事件可以委托给父级处理

好处： (1)提高性能  **(2)可以给未来的子元素添加事件**

## 11. 事件源
- oEvent.target
  *兼容：高级浏览器
  低级浏览器 -> undefined

- oEvent.srcElement
  *兼容：Chrome、IE系
  FF -> undefined

**兼容写法**
var oSrc = oEvent.srcElement || oEvent.target;

> 注意：oSrc.tagName获取到的标签名都是大写

###给子级循环添加事件 闭包的替代写法 -> 委托
``` javascript
<script>
	oUl.onclick = function(ev) {
		var oEvent = ev || event;
		var oSrc = oEvent.scrElement || oEvent.target;
		if (oSrc.tagName == 'LI') {	// **注意获取到的标签名都是大写
			this.style.background = '#f00';
		}
	}
</script>
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)