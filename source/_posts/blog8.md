---
title: 每天10个前端知识点：各种宽高距离篇
date: 2017-01-20 03:27:35
categories: 前端札记
tags: [js]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

{% aplayer "Wonderful u" "AGA" "http://ojvx9eehr.bkt.clouddn.com/AGA%20-%20Wonderful%20u.mp3" %}


---
*这几个宽高距离总是让我辨别不清，索性整合对比了下，方便记忆。*


---

## 1. 滚动距离
**html简写：document.documentElement**
- document.body.scrollTop
	兼容：Chrome
	其他 -> 0
- document.documentElement.scrollTop
	兼容：IE、FF
	其他 -> 0

**兼容写法**
纵向：var scrollT = document.documentElement.scrollTop || document.body.scrollTop;
横向：var scrollL = document.documentElement.scrollLeft || document.body.scrollLeft;

<!-- more -->

## 2. 滚动高度
obj.scrollHeight
- 内容高度 > 盒模型高度    取内容高度
- 盒模型高度 > 内容高度    去盒模型高度

## 3. 可视区高度
- 可视区高度：var clientH = document.documentElement.clientHeight;
- 可视区宽度：var clientW = document.documentElement.clientWidth;

兼容：全兼容

## 4. 物体高度
**获取的是盒模型大小 = width/height + padding + border;**

- 物体的高度：var oH = obj.offsetHeight;
- 物体的宽度：var oW = obj.offsetWidth;

> 注意：offsetWidth/height只有append进body后才有，创建时获取不到盒模型的大小。

## 5. offsetHeight && getStyle()

| 	   	| 		offsetHeight      |     getStyle() 	 		 |
| :--: 	| 	   :----: 	          |   :----: 	 		     |
|返回值 | 	   数字  	  		  |   		字符串 		     |
|获取值 | 获取的是盒模型的大小(width/height+padding+border)  |   获取的是纯width/height			 |
| display:none后   | 	   0    |   仍可以获取 				 |

## 6. 物体的相对距离
- 物体距离定位父级左边距离：var oL = obj.offsetLeft;
- 物体距离定位父级上边距离：var oT = obj.offsetTop;

## 7. 关于父级
- 结构父级 obj.parentNode    根：document
- 定位父级 obj.offsetParent  根：body

## 8. 封装一个物体距离左边/上边的绝对位置的函数
``` javascript
<script>
	function getPos(obj) {
		var l = 0;	// 距离左边的绝对距离
		var t = 0;	// 距离上边的绝对距离
		while(obj) {
			l += obj.offsetLeft;
			t += obj.offsetTop;
			obj = obj.offsetParent;	// 继续查找上一层定位父级
		}

		return {left: l, top: t};
	}
</script>

```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)