---
title: 每天10个前端知识点：HTML5(选择器、自定义属性、存储)
date: 2017-03-19 03:27:35
categories: 前端札记
tags: [HTML5]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。



---

- H5选择器补充
	- querySelectorAll 对比 getElements 的优势
	- jQuery的选择器即是querySelectorAll
- H5自定义属性 dataset
- H5元素类名操作 classList
	- 隐式原型上的方法\(不一一列举\)
- H5本地存储 localStorage
	- Web Storage实际上由两部分组成：sessionStorage与localStorage
	- sessionStorage与localStorage操作相同

<!-- more -->

---

## 3. H5选择器补充
*兼容：IE8+

- document/dom.querySelector() 匹配指定 css 选择器的一个元素

- querySelectorAll()  匹配指定 css 选择器的所有元素 (NodeList)

> **注意：** querySelectorAll()方法得到的类数组对象是非动态实时的

### querySelectorAll 对比 getElements 的优势
可以操作数组

``` html
	<div class="box"></div>
	<div class="box"></div>
	<div class="box"></div>
```

``` javascript
<script>
	var aBox = document.querySelectorAll('div');

	// getElements得到的是伪数组，不能操作各项的属性

	// var aBox = document.getElementsByTagName('div');
	// for (var i = 0; i < aBox.length; i++) {
	// 	aBox[i].onclick = function() {
	// 		alert(1);
	// 	}
	// }

	aBox.forEach(function(item, index) {
		item.onclick = function() {
			alert(index);
			// 弹出当前点击div的索引值，依次为0、1、2
		}
	})
</script>
```


### jQuery的选择器即是querySelectorAll

``` javascript
<script>
	function $(selector) {
		var items = document.querySelectorAll(selector);

		if(items.length > 1) {
			return items;
		} else if (items.length == 0) {
			return;
		} else {
			return items[0];
		}
	}
</script>
```

## 4. H5自定义属性 dataset

[HTML5自定义属性对象Dataset简介](http://www.zhangxinxu.com/wordpress/2011/06/html5%E8%87%AA%E5%AE%9A%E4%B9%89%E5%B1%9E%E6%80%A7%E5%AF%B9%E8%B1%A1dataset%E7%AE%80%E4%BB%8B/)

[HTML5 datalist在实际项目中应用的可行性研究](http://www.zhangxinxu.com/wordpress/2013/03/html5-datalist-%E5%AE%9E%E9%99%85%E5%BA%94%E7%94%A8-%E5%8F%AF%E8%A1%8C%E6%80%A7/)

示例
``` html
	<a data-link="#" data-user-name="chloe">wangchloe.vip</a>
```

``` javascript
<script>
	var oA = document.querySelector('a');
	oA.dataset.link = 'http://wangchloe.vip';
	oA.href = oA.dataset.link + '?name=' + oA.dataset.userName;
	// http://wangchloe.vip?name=chloe
	// *注意：两个及以上属性名调用时需转化为驼峰命名
</script>
```

## 5. H5元素类名操作 classList

[HTML5 DOM元素类名相关操作API classList简介](http://www.zhangxinxu.com/wordpress/2013/07/domtokenlist-html5-dom-classlist-%E7%B1%BB%E5%90%8D/)

``` html
<!-- 该示例来源于张鑫旭个人博客 -->
<body class="a b c"></body>
```

``` javascript
<script>
	console.log(document.body.classList);
	console.log(document.body.className);
	console.log(document.body.classList.toString() === document.body.className;)	// true
</script>
```

> HTML5 DOM元素类名相关操作API classList简介>>>测试

![HTML5 DOM元素类名相关操作API classList简介>>>测试](http://img.blog.csdn.net/20170110000715296?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdzk1MDkxNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 隐式原型上的方法(不一一列举)

- obj.add(cName1, cName2, ...);

- obj.remove(cName1, cName2, ...);

- obj.toggle(cName);

- obj.contains(cName);

## 6. H5本地存储 localStorage

[HTML5 localStorage本地存储实际应用举例](http://www.zhangxinxu.com/wordpress/2011/09/html5-localstorage%E6%9C%AC%E5%9C%B0%E5%AD%98%E5%82%A8%E5%AE%9E%E9%99%85%E5%BA%94%E7%94%A8%E4%B8%BE%E4%BE%8B/)

**cookie && localStorage && sessionStorage** (来源CSDN)

- 共同点：
     都是保存在浏览器端，且同源的。

- 不同点：
	1. cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递。而sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。

	2. cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下。

	3. 存储大小限制也不同，cookie数据不能超过4k，同时因为每次http请求都会携带cookie，所以cookie只适合保存很小的数据，如会话标识。sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。

	4. 数据有效期不同，sessionStorage：仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持；localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。

	5. 作用域不同，sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localStorage 在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的。

	6. Web Storage 支持事件通知机制，可以将数据更新的通知发送给监听者。Web Storage 的 api 接口使用更方便。

### Web Storage实际上由两部分组成：sessionStorage与localStorage

- sessionStorage用于本地存储一个会话(session)中的数据，这些数据只有在同一个会话中的页面才能访问并且当前会话结束后数据也随之销毁。因此sessionStorage不是一种持久化的本地存储，仅仅是会话级别的存储。

- localStorage用于持久化的本地存储，除非主动删除数据，否则数据是永远不会过期的。

> *兼容：可以先测试，以确定window.localStorage是否存在。

### sessionStorage与localStorage操作相同

- 设置

`localStorage.key = value;`

`localStorage.setItem(key, value);`

``` javascript
<script>
	var userName = 'chloe';

	//存储，IE6~7 cookie 其他浏览器HTML5本地存储
	if (window.localStorage) {
		localStorage.setItem("name", userName);
	} else {
		Cookie.write("name", userName);	// MooTools框架下cookie的写法
	}
</script>
```

- 读取

`localStorage.key;`

`localStorage.getItem(key);`

``` javascript
<script>
	var userName = window.localStorage ? localStorage.getItem("name") : Cookie.read("name"); // MooTools框架下cookie的写法
</script>
```

- 遍历

``` javascript
<script>
	var storage = window.localStorage;
	for (var i = 0, len = storage.length; i < len; i++) {
		var key = storage.key(i);
		var value = storage.getItem(key);
		console.log(key + "=" + value);
	}
</script>
```

- 删除

`delete localStorage.key;`

`localStorage.removeItem(key);`

``` javascript
<script>
	if (window.localStorage) {
		localStorage.removeItem("name");
	} else {
		Cookie.dispose('name'); // MooTools框架下cookie的写法
	}
</script>
```

- 清空

`localStorage.clear();`

- 监听
`window.onStorage` 监听localStorage变化

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)