---
title: 每天10个前端知识点：原生篇(5)
date: 2017-01-16 03:04:50
categories: 前端札记
tags: [js]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。



---
*本次内容是一些比较零散的知识点，有坑慎入，踩坑快乐！*

---

## 1. select下拉框的相关属性
- 选中的索引值
oSel.selectedIndex
- 获取所有选项
oSel.options
- 获取选中的选项的文本内容
oSel.options[oSel.selectedIndex].innerHTML
oSel.options[oSel.selectedIndex].text

<!-- more -->

- 添加选项
var option = new Option(文本内容, value值);
oSel.options.add(option);
- 删除选项
oSel.options.remove(index);


## 2. 预解析
变量和函数的定义会预先解析，解析在当前script标签内的最上面
> 作用域：(1)script (2)函数

``` javascript
<script>
	var a = 111;
	function show() {
		alert(a);
		var a = 12;
		// 相当于
		// var a;
		// alert(a);	//undefined
		// a = 12;
	}
	show();		// undefined
	alert(a);	// 111
</script>
```

## 3. 已知计算机编码查看对应单词
String.fromCharCode('0x4e00');	 // 对应中文 "一"
- 第一个中文 	0x4e00 一
- 最后一个中文	0x9fa5 龥(yu)

**unicode编码：以\u开头 \u4e00(一) ~ \u9fa5(龥)**

## 4. 字节长度和编码的关系
英文、数组都占1个字节，与编码格式无关
UTF-8；中文占3个字节
GB2312：中文占2个字节

### 封装一个求字节长度的函数
``` javascript
<script>
	function getBytesLen(str, type) {	// 字符串，编码格式
		var result = 0;
		type = type.toLowerCase();
		for (var i = 0; i < str.length; i++) {
			if(str.charAt(i) >= '\u4e00' && str.charAt(i) <= '\u9fa5') {
				if(type == 'gb2312') {
					result += 2;
				} else {
					result += 3;
				}
			} else {
				result++;
			}
		}
		return result;
	}
</script>
```

## 5. 严格模式
**非严格模式下，没有用var定义变量时为全局变量，全局的东西都属于window**
``` javascript
<script>
	function show(){
		a = 12;		//a没有var时a为全局变量
		alert(a);	//1. 12
	}
	show();		//需先调用show函数
	alert(a);	//2. 12
</script>
```
严格模式

在script标签内最上面写` 'use strict'; `   **IE6不识别但不报错**

`'use strict';`好处：
1. 修复局部this的问题
2. 不允许if/while/for里面定义函数
3. 去掉了with(){}
4. 定义变量必须加var

作用域：(1)当前script标签内 (2)函数  (3)js文件


**坑**
``` javascript
<script>
	var a = 5;
	if(a % 2) {
		function show() {
			alert('单数');
		}
	} else {
		function show() {
			alert('双数');
		}
	}
	show();	// 单数 最新版高级浏览器中预解析不会覆盖，之前版本预解析后此例预解析覆盖 弹出双数
</script>
```

``` javascript
<script>
	'use strict';
	var a = 5;
	if(a % 2) {
		function show() {
			alert('单数');
		}
	} else {
		function show() {
			alert('双数');
		}
	}
	show();	// show is not defined，严格模式不允许if/for里面定义函数
</script>
```

## 6. 连等及逗号运算符
### 连等
**函数内部使用连等定义变量，第一个是局部变量，其余是全局变量。**

``` javascript
<script>
	function show(){
		var a=b=c=1;	//a是局部变量，b、c是全局变量
	}
</script>
```

### 逗号运算符
**逗号运算符 以最后一个为准**

``` javascript
<script>
	var a=(1,2,3);	// a=3

	for(var i=0, j=5, k=8; i<10, j<10, k<10; i++, j++, k++) {

	}
	alert(i+j+k);	// 2+7+10=19
</script>
```

## 7. 文本提示框
聚焦事件：oT.onfocus = function() {};
失焦事件：oT.onblur = function() {};
> 强制获取一个焦点：oT.focus();
> 强制失去一个焦点：oT.blur();

## 8. form表单
想要提交数据须有
1. action 提交的地址 `<form action=''></form>`
2. name   数据名称   `<input name="user.tel" />`
3. value  数据       `input.value`

提交方式
1. get(默认) 容量32K左右  不安全，有缓存
** 好处：(1)分享 (2)收藏**

2. post      容量1G左右   相对安全，没有缓存

> 缓存(cache)
对于浏览器而言，相同的地址只会访问一次


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)