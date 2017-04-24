---
title: 每天10个前端知识点：原生篇(6)
date: 2017-02-07 03:27:35
categories: 前端札记
tags: [js]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

{% aplayer "I Miss You" "Czarina" "http://ojvx9eehr.bkt.clouddn.com/%21Czarina%20-%20I%20Miss%20You.mp3" %}

---


*本次内容：cookie、Require、数据交互前导*


---

## 1. cookie

**数据类型：string**

1. 需在服务器环境中
2. 不安全

<!-- more -->

3. 过期时间(expires)：默认关闭浏览器时清空
  > session的常见实现形式是会话cookie（session cookie），即未设置过期时间的cookie;
  平常所说的cookie主要指的是另一类cookie——持久cookie（persistent cookies）。

  ``` javascript
  <script>
    var oDate = new Date();
    oDate.setDate(oDate.getDate()+3);  // 延迟三天清空
    document.cookie = 'expires=' + oDate;
  </script>
  ```
4. 容量小，4k左右
5. 返回的数据类型是字符串，使用时需转化
6. 内部能访问外部cookie，外部不能访问内部cookie
   path统一设置到根目录  `document.cookie = 'name=value;path=/';`

7. domain 设置域名访问
	eg: baidu
	baidu.com  `domain=baidu.com`  需在上一级域名设置domain
	fanyi.baidu.com  `setCookie`
	baike.baidu.com  `getCookie`

8. 有缓存

### 封装cookie.js
``` javascript
<script>

	// 设置cookie
	function setCookie(name, value, iDay) {
		if(iDay) {
			var oDate = new Date();
			oDate.setDate(oDate.getDate() + iDay);
			document.cookie = name + '=' + value + ';path=/;expires=' + oDate;
			// document.cookie = name + '=' + value + ';path=/;expires=' + oDate + ';domain=localhost';  // 设置只能在localhost下设置cookie
		} else {
			document.cookie = name + '=' + value + ';path=/';  // 关闭浏览器时清空
		}
	}

	// cookie形式 eg: "name1=value1; name2=value"  (表达式之间有空格)

	// 获取cookie
	function getCookie(name) {
		var str = document.cookie;
		var arr = str.split('; ');  // **分号后有空格
		for(var i = 0; i < arr.length; i++) {
			var arr2 = arr[i].split('=');
			if(arr2[0] == name) {
				return arr2[1];
			}
		}

		return '';  // 没有找到即返回空字符串
	}

	// 移除cookie
	function removeCookie(name) {
		setCookie(name, 'xxx', -10);  // **将日期设为小于0的值  '-0'不能达到移除cookie的目的
	}
</script>
```

## 2. 模块化

### sea.js && require.js
| sea.js | 		require.js      |
| :--: | 	   :----: 	  |
| 采用CMD(通用模块定义,依赖就近)    | 	   采用AMD(异步模块定义,依赖前置)  	  |

[AMD 和 CMD 的区别有哪些? - 玉伯的回答 - 知乎](https://www.zhihu.com/question/20351507/answer/14859415)

[前端模块化（CommonJs,AMD和CMD）](http://www.jianshu.com/p/d67bc79976e6)

[详解JavaScript模块化开发](https://segmentfault.com/a/1190000000733959)

## 3. Require.js

[RequireJS 中文网](http://www.requirejs.cn/)

**好处**
1. 解决命名冲突
2. 解决文件彼此依赖
3. 自动引入js
4. 异步加载，可维护性高
   只发送一个请求，最终引用文件命名为init.js
   或者`<script src="require.js" data-main="init"></script>`

#### (1)定义模块

r1.js
``` javascript
<script>

	define(function(require, exports, module) {
		// 引入模块，导出模块，批量导出
		exports.a = 1;

		// console.log(1);

		// return {a:1, b:2};
	});

</script>
```

#### (2)使用模块
``` javascript
<script>

	// require(['r1.js']);  // 可不写函数

	require(['r1.js'], function(mod) {  // **使用时注意路径，若报错试着将路径改为'js/r1.js'
		console.log(mod.a);	// 1
	})

</script>
```

``` javascript
<script>

	require(['r1.js', 'r2.js'], function(mod1, mod2) {	// 使用多个模块
		console.log(mod1.a, mod2.a);
	})

</script>
```

#### (3)引用模块

这里的模块依赖其实应该一开始就写好。

![依赖前置](http://ojvx9eehr.bkt.clouddn.com/img/%E4%BE%9D%E8%B5%96%E5%89%8D%E7%BD%AE.PNG)

``` javascript
<script>

	define(function(require, exports, module) {
		// 引入模块，导出模块，批量导出
		var mod1 = require('r1.js');
		var mod2 = require('r2.js');
		exports.sum = function() {
			return mod1.a + mod2.a;
		}
	});

</script>
```

> 因专用于js，require时可省略类型后缀  'js/r1.js' -> 'js/r1'


### Require的使用结构一般如下
exp1.html放于主目录
js文件放在js文件夹下

exp1.html
```
...
...

<script src="js/require.js" data-main="js/init"></script>

...
...


```

init.js

``` javascript
<script>

	require(['exp1']);

</script>
```

exp1.js

``` javascript
<script>

	define(function(require) {
		var r1 = require('r1');

		...
		...

		oBtn.onclick = function() {
			...
			r1(xxx);
		}
	})

</script>
```

r1.js
``` javascript
<script>

	define(function(require) {
		var move = require('move');

		...
		...

		return function(yyy) {
			...

			...

			move(aaa, {opacity: 1});
		}
	})

</script>
```

## 4. 数据交互

### form提交数据
缺点：(1)会刷新页面  (2)不能取出数据

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


### ajax(Asynchronous JavaScript and XML，异步JavaScript和XML)
1. 需在服务器环境中
2. 编码需一致
3. url若为文件名，可不写文件名后缀
4. 返回的数据类型是字符串，使用时需转化
5. 缓存(get方法)
  浏览器清除缓存 `ctrl+F5` 或 `ctrl+alt+delete`
  防止IE缓存
    路径+随机因子
    (1)`'路径?t=' + Math.random();`
    (2)`'路径?t=' + oDate.getTime();`
    eg:`var URL = 'user.php?act=login&user=' + logU.value + '&pass=' + logP.value + '&t=' + new Date().getTime();`

## 5. eval的替代用法
``` javascript
<script>

	function strTrs(str) {
		var fn = new Function('return' + str);
		return fn();
	}

</script>
```

## 6. 数据交互时输入中文问题
中文转换URL编码 `encodeURIComponent(str)`
解编码 `decodeURIComponent(str)`

**IE兼容写法**
``` javascript
<script>

	var URL = 'user.php?act=login&user=' + encodeURIComponent(logU.value) + '&pass=' + encodeURIComponent(logP.value) + '&t=' + new Date().getTime();

</script>
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)