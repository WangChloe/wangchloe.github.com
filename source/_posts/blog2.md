---
title: 每天10个前端知识点：原生篇(2)
date: 2017-01-12 21:00:28
categories: 前端札记
tags: [js]

---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。




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

加载 -> 解析 -> 渲染

###  加载 
1. 用户输入网址（假设是个html页面，并且是第一次访问），浏览器向服务器发出请求，服务器返回html文件，浏览器会会”自上而下“加载，并在加载过程中进行解析渲染； 
2. 浏览器开始载入html代码，发现＜head＞标签内有一个＜link＞标签引用外部CSS文件； 
3. 遇到浏览器又发出CSS文件的请求获取CSS，服务器返回这个CSS文件； 
4. 浏览器继续载入html中＜body＞部分的代码，并且CSS文件已经拿到手了，可以开始渲染页面了； 
5. 浏览器在代码中发现一个＜img＞标签引用了一张图片，**这是异步请求**，并不会影响html文档进行加载;
6. 服务器返回图片文件，由于图片占用了一定面积，影响了后面段落的排布，因此浏览器需要回过头来重新渲染这部分代码； 
7. 浏览器发现了一个包含一行Javascript代码的＜script＞标签，html文档会挂起渲染（加载解析渲染同步）的线程，不仅要等待文档中js文件加载完毕，还要等待解析执行完毕，才可以恢复html文档的渲染线程； 
8. Javascript脚本执行了这条语句，它命令浏览器隐藏掉代码中的某个＜div＞ （style.display=”none”）。突然少了这么一个元素，浏览器不得不重新渲染这部分代码； 
9. 终于等到了＜/html＞的到来，浏览器泪流满面…… 
10. 等等，还没完，用户点了一下界面中的“换肤”按钮，Javascript让浏览器换了一下＜link＞标签的CSS路径； 
11. 浏览器召集了在座的各位＜div＞＜span＞＜ul＞＜li＞们，“大伙儿收拾收拾行李，咱得重新来过……”，浏览器向服务器请求了新的CSS文件，重新渲染页面。

> ps:
> - JS 会阻塞后续 DOM 解析以及其它资源(如 CSS，JS 或图片资源)的加载。
> - CSS不阻塞DOM的加载和解析（它只阻塞DOM的渲染呈现。这里谈加载），不会阻塞其它资源(如图片)的加载，但是会阻塞 后续JS
> - 执行顺序的原因其一：js执行代码可能会依赖到css样式。css只阻塞执行而不阻塞js的加载。

### 解析
1. 浏览器通过请求的 URL 进行域名解析，向服务器发起请求，接收文件（HTML、CSS、JS、Images等等）
2. HTML 文件加载后，开始构建 DOM Tree（DOM树）
3. CSS 样式文件加载后，开始解析和构建 CSS Rule Tree
4. Javascript 脚本文件加载后， 通过 DOM API 和 CSSOM API 来操作 DOM Tree 和 CSS Rule Tree

### 渲染

1. 浏览器引擎通过 DOM Tree 和 CSS Rule Tree 构建 Rendering Tree（渲染树）
2. 布局阶段——在屏幕上绘制渲染树中的所有节点的几何属性，比如： 位置，宽高，大小等等，这个过程称为 Flow 或 Layout 
3. 绘制元素——绘制所有节点的可视属性。
4. 合并渲染层——把以上绘制的所有图层（类似于PhotoShop中的“图层”）合并,最终输出一张图片

#### 如何优化浏览器渲染过程
1. 创建有效的 HTML 和 CSS ，不要忘记指定文档编码，比如 `<meta charset="utf-8">`
2. CSS 样式应该包含在 < head >中， Javascript 脚本出现在`< body >` 末尾。
3. 减少 CSS 嵌套层级和选择适当的选择器。
4. 不要通过 JS 逐条修改 DOM 的样式，提前定义好 CSS 的 Class 进行操作。
5. 尽量减少将 DOM 节点属性值放在循环当中，会导致大量读写此属性值。
6. 尽可能的为产生动画的 HTML 元素使用 fixed 或 absolute 的 position ，那么修改他们的 CSS 是不会 Reflow 的。

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
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)