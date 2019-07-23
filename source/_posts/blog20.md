---
title: 每天10个前端知识点：正则应用篇
date: 2017-02-18 03:27:35
categories: 前端札记
tags: [js, 应用]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。



---

## 1. 校验座机号码
示例：
021-8888888
0791-88888888

- 区号-  ->  `(0[1-9]\d{1,2}-)?`    3/4位数字  首位数字为0、第二位非0  区号-可有可无
- 号码   ->  `[1-9]\d{6,7}`         7/8位数字  首位数字非0

正则：
`/^(0[1-9]\d{1,2}-)?[1-9]\d{6,7}$/`

<!-- more -->

## 2. 校验邮箱
示例：
wangchloe@yeah.net
123123123@163.com.cn
用户名 + @ + 域名 +      . + 后缀
`\w+      @   [a-z0-9-]+ (\.[a-z]{2,8}){1,2}

正则：
`/^\w+@[a-z0-9-]+(\.[a-z]{2,8}){1,2}$/`

## 3. 校验年龄(18-100)
18-19    20-99     100
1[89] |  [2-9]\d | 100

正则：
/^1[89]|[2-9]\d|100$/
> ^优先级比|高，会先执行^1[89]和100$，并且^与超过两个|同时出现就会错乱，需要加括号包裹多个条件，提升优先级

`/^([89]|[2-9]\d|100)$/`

## 4. 仿trim()去除首尾空格
- 去首空格  `^\s+`
- 去尾空格  `\s+$`

去掉开头为空格或者空格为结尾

正则：
`/^\s+|\s+$/`

## 5. 校验名字
`str.fromCharCode('0x4e00')`
第一个中文 0x4e00  一
最后一个中文  0x9fa5  龥(yu)

- unicode编码：统一编码 utf-8 utf-16 utf-32
以\u开头  \u4e00(一) ~ \u9fa5(龥)
- GB2312编码

两个到七个汉字
正则：
`/^[\u4e00-\u9fa5]{2,7}$/`

## 6. 首字母大写
```
<script>
	var str = 'this is wangchloe';

	var str2 = str.replace(/\w+/g, function(s) {
		return s.charAt(0).toUpperCase() + s.substring(1);
	});

	console.log(str2);  // This is wangchloe
</script>
```

## 7. 过滤标签
```
<script>
	var str = oBox.innerHTML;
	var str2 = str.replace(/<[^>]+>/g, '');

	console.log(str2);
</script>
```

## 8. 正则getByClass等方法

```
<script>
	function getByClass(oParent, sClass) {  // oParent为从哪个父级下面查找类为sClass的元素
		if(oParent.getElementsByClassName) {  // IE8- -> undefined  高级浏览器 -> function
 			return oParent.getElementsByClassName(sClass);  // 高级浏览器
		} else {	// IE8
			var arr = [];
			var aEle = oParent.getElementsByTagName('*');
			var reg = new RegExp('\\b' + sClass + '\\b');
			for (var i = 0; i < aEle.length; i++) {
				if(reg.test(aEle[i].className)) {
					arr.push(aEle[i]);
				}
			}
			return arr;
		}
	}

	function hasClass(obj, sClass) {
		var reg = new RegExp('\\b' + sClass + '\\b');
		return reg.test(obj.className);
	}

	function addClass(obj, sClass) {
		if(obj.className) {
			if(!hasClass(obj, sClass)) {
				obj.className += ' ' + sClass;
			}
		} else {
			obj.className = sClass;
		}
	}

	function removeClass(obj, sClass) {
		var reg = new RegExp('\\b' + sClass + '\\b');
		if(hasClass(obj, sClass)) {
			// obj.className = obj.className.replace(reg, '').replace(/^\s+/g, '');
			obj.className = obj.className.replace(reg, '').replace(/\s+/g, ' ').replace(/^\s+|\s+$/, '');
		}
	}
</script>
```

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)