---
title: 每天10个前端知识点：原生篇(3)
date: 2017-01-13 02:51:22
categories: 前端札记
tags: [js]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。




---
*本次内容主要关于js函数返回值、定时器、日期(Date)对象、this以及闭包。*

---

## 1. 返回值问题(return)
1. return语句后面的代码不执行
1. **函数若没有写return，则默认返回undefined**
1. **函数返回语句为return; 也返回undefined**
1. **return必须写在函数function内**


<!-- more -->

## 2. undefined出现的情况
- 函数没有返回值或只有return;
- 定义了一个变量，但没有赋值

eg: 
``` javascript
<script>
  var a; // undefined
  function show(a) {}
  show();	// undefined`
</script>
```

- 访问不存在的属性

> eg: oDiv.aaa;	// undefined

## 3. eval(字符串)
> 虽然这个不建议使用，但还是聊聊这个东西是怎么用的吧

eval能把字符串里面的代码转换成js能理解的程序，把引号中的拿出来运行

``` javascript
<script>
	var a = '[1, 2, 3]';	//字符串
	alert(eval(a));		// 1, 2, 3  '[1, 2, 3]' -> [1, 2, 3]
</script>
```

## 4. 数字小于10的补零函数
``` javascript
<script>
	function toTen(num) {
		if(num < 10) {
			return '0' + num;
		} else {
			return '' + num;	// 函数的返回类型最好保持一致
		}
	}
</script>
```
简化
``` javascript
<script>
	function toTen(num) {
		return num < 10 ? '0' + num : '' + num;
	}
</script>
```

## 5. 定时器
### (1) Interval(每过一段时间执行一次，循环执行)
- 开启定时器
setInterval(函数/函数名, 时间);

> 时间单位是毫秒

- 关闭定时器
clearInterval(定时器的名字);

**interval的问题**

1.时间不能设置太小的值

 eg:设置0其实会超出0
``` javascript
<script>
	var a = 6;
	setTimeout(function(){
		a = 66;
	},0);
	alert(a);	//6
</script>
```

2.时间值越小越不稳定

3.打开其他窗口时，该窗口定时器时间会变长

> *定时器的最佳时间：30ms (时间过小，程序性能开销大)

### (2) Timeout(过一段时间执行一次，只执行一次)
- 开启定时器
setTimeout(函数/函数名, 时间);

> 时间单位是毫秒

- 关闭定时器
clearTimeout(定时器的名字);

``` javascript
	<script>
		// 定时器先关后开
		var bSin = false;
		var timer = setInterval(function() {
			if(bSin) {
				return;
			}
			bSin = true;
		}, 30);

		function clear() {
			clearInterval(timer);
			bSin = false;
		}
	</script>
```

** **

## 6. 日期对象
``` javascript
<script>
	// 获取时间
	var oDate = new Date();
	oDate.getFullYear();	// 获取年
	oDate.getMonth();		// 获取月，从0开始，获取+1，设置-1 !important
	oDate.getDate();		// 获取日
	oDate.getDay();			// 获取星期，星期天 -> 0，星期一 ~ 星期六 -> 1 ~ 6

	oDate.getHours();		// 获取小时，记得加s，下同 !important
	oDate.getMinutes();		// 获取分钟
	oDate.getSeconds();		// 获取秒
	oDate.getMillseconds();	// 获取毫秒
	oDate.getTime(); 		// 时间戳 当前时间距离1970/1/1凌晨的毫秒数
</script>
```

``` javascript
<script>
	// 设置时间
	var oDate = new Date();
	oDate.setFulllYear(2017, 11, 13);	// 设置年、月、日  月份设置时-1
	oDate.setHours(0, 0, 0, 0);			// 设置时、分、秒、毫秒

	// 获得时间戳
	oDate.getTime();	// 设置后的时间距离1970/1/1凌晨的毫秒数
</script>
```

## 7. 日期对象应用
> oDate.setDate(31); // 假设本月有30天会跑到下个月的第一天 会自动进位
> oDate.setDate(0); // 会跑到上个月的最后一天

### 本月有多少天
``` javascript
<script>
	var oDate = new Date();
	oDate.setMonth(oDate.getMonth() + 1); // 当前月份+1
	oDate.setDate(0);	// setDate(0);
	alert(oDate.getDate());

</script>
```
### 本月第一天是周几
``` javascript
<script>
	var oDate = new Date();
	oDate.setDate(1);	// setDate(1);
	alert(oDate.getDay());
</script>
```

### 本月最后一天是周几
``` javascript
<script>
	var oDate = new Date();
	oDate.setMonth(oDate.getMonth()+1);	// 当前月份+1
	oDate.setDate(0);	// setDate(0);
	alert(oDate.getDay());
</script>
```

## 8. 事件函数相同可以合并
eg: oDiv1.onmouseout = oDiv2.onmouseout = function() {};

## 9. this
this: 当前方法属于谁，this就是谁
**this默认属于window**

定时器里的this不能直接使用，原因：this指向了window

### (1) 定时器中的this不指向元素，指向window
解决：在定时器外保存this
``` javascript
<script>
oBtn.onclick = function() {
	var _this = this;
	setTimeout(function(){
		_this.style.background = '#f00';
	},1000);
}
</script>
```
### (2) 调用封装函数使用this，this不指向元素，指向window
### (3) 低级浏览器attachEvent)事件绑定里面的this 报错

## 10. 闭包
用处：
1. 解决变量名冲突
2. 解决循环添加事件，事件中的循环变量不能用的问题

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
闭包写法：

``` javascript
<script>
	function clickEg() {
		//获取一组按钮	ps:js一组元素不能一起改变样式或设置事件
		var aBtn = document.getElementsByTagName('button');
		for(var i = 0; i < 3; i++) {
			(function(index) {
				aBtn[i].onclick = function() {
					aBtn[index].style.background = 'f00';
				}
			})(i);
		}
	}
</script>

```

---

``` javascript
<script>
	for(var i=0; i<2; i++) {
		setTimeout(function(){
			alert(i);
		}, 2000);
	}	// 结果：两秒后alert两次2，两秒后i已为2，然后执行两次循环
</script>

```
闭包写法：

``` javascript
<script>
	for(var i=0; i<2; i++) {
		(function(a){
			setTimeout(function(){
				alert(a);
			},2000);
		})(i);
	}	// 结果：两秒后alert 0、1
</script>

```

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)
