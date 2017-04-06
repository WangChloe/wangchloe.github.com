---
title: 每天10个前端知识点：jQuery(下)
date: 2017-02-16 03:27:35
categories: 前端札记
tags: [jQuery]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

{% aplayer "你说" "赵乃吉" "http://ojvx9eehr.bkt.clouddn.com/%E8%B5%B5%E4%B9%83%E5%90%89%20-%20%E4%BD%A0%E8%AF%B4.mp3" %}

---

## 1. jQuery获取物体信息
### js
- obj.offsetWidth
- obj.offsetHeight
- obj.offsetLeft
- obj.offsetTop

- obj.parentNode  结构父级  根：document
- obj.offsetParent  定位父级  根：body

**js处理小数较弱，取出时取整Math.round()**


<!-- more -->

### jQuery(只封装了以下方法)
- `obj.width();`  纯width
- `obj.height();`  纯height

- `obj.outerWidth();`  盒子模型的width  (width+padding+border)  =>  offsetWidth
- `obj.outerHeight();`  盒子模型的height  (height+padding+border)  => offsetHeight

- `obj.position().left;`  距离定位父级left值  (不包括margin)   => offsetLeft
- `obj.position().top;`   距离定位父级top值   (不包括margin)   => offsetTop

- `obj.offset().left;`  距离定位父级left值  (包括margin)
- `obj.offset().top;`  距离定位父级top值  (包括margin)

- `obj.scrollTop();`  元素相对滚动条顶部的偏移
- `obj.scrollLeft();`  元素相对滚动条左侧的偏移

父级
- `obj.parent()`  结构父级  根：document    => parentNode
- `obj.parents()`  返回被选元素的所有祖先元素，直到`<html>`
- `obj.offsetParent()`  定位父级  根：body  => offsetParent

子级
- `obj.children()`  返回被选元素的所有直接子元素
- `obj.find()`      返回被选元素的后代元素，一路向下直到最后一个后代

## 2. jQuery筛选
### (1) 过滤
- `.eq(index)`
  - index >= 0   正向选取(0代表第一个，1代表第二个)
  - index < 0    反向选取(-1代表倒数第一个)

- `.first()`
- `.last()`
- `.hasClass(className)`

### (2) 查找
- `.find(tagName/className/id)`  eg: `oBox.find('ol li');`

## 3. jQuery <=> js
### (1) 原生js转jQuery对象
**$() 包裹**
this -> $(this)
document -> $(document)

```
<script>
	var oDiv = document.getElementById('div1');
	$(oDiv).html('xxx');
</script>
```

### (2) jQuery转原生js对象
**[] / get()**

1. `$('div')[0].innerHTML = 'xxx';`
2. `$('div').get(0).innerHTML = 'xxx;`

## 4. jQuery链式运动
`obj.css(...).html(...).attr(...).click();`

## 5. jQuery循环
`obj.each(function(){...});`

[JS中的forEach、$.each、map方法推荐](http://www.jb51.net/article/81955.htm)

eg:
```
<script>
	$('div').each(function(index, element) {  // 索引，当前元素
		console.log($(element).html);  // **element是原生对象，需转成jq对象
		$(this);  // 当前对象
	});
</script>
```
## 6. jQuery工具
- `$.trim(str);`  去掉字符串起始和结尾的空格
- `$.browser.version;`  浏览器版本

eg:
```
<script>
	if($.browse.version.substring(0, 1) == '6') {
		// IE6 code here
	}
</script>
```

## 7. jQuery Ajax
`$.ajax({...});`

- **type: 'get/post'**

```
<script>
	$.ajax({
		url: URL,
		data: {},
		type: 'get',
		error: fn,
		complete: fn,
		time: 3000;
		success: function(str) {
			console.log(str);
		}
	})
</script>
```

## 8. jQuery jsonp
`$.ajax({...});`

- **dataType: 'jsonp'**
- **cbName: 'callback/cb'**

```
<script>
	$.ajax({
		url: URL,
		data: {},
		dataType: 'jsonp',
		cbName: 'callback',
		success: function(json) {
			console.log(json);
		}
	});
</script>
```

## 9. jQuery插件
### 写插件
$: jq
fn: 帮助

**jq里面除了插件里的this以外，其他都是原生的js**

[jquery的$.extend和$.fn.extend作用及区别](http://blog.sina.com.cn/s/blog_7c5d61f30101da1k.html)

### 一个插件
`$.fn.插件名 = fn;`

```
<script>
	$.fn.插件名 = function() {
		this.css('name', 'value');  // 插件中的this不用加$
	}

	$('div').插件名();
</script>
```

### 一组插件
`$.fn.extend(...);`

> 插件调用不能用链式

```
<script>
	$.fn.extend({
		插件名1: function() {
			this.css('name', 'value');
		},
		插件名2: function() {
			this.css('name', 'value');
		}
	})

	// 插件调用不能用链式
	$('div').插件名1();
	$('div').插件名2();
</script>
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)