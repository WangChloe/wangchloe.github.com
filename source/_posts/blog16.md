---
title: 每天10个前端知识点：ajax && jsonp
date: 2017-02-13 03:27:35
categories: 前端札记
tags: [js]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

{% aplayer "乞丐" "回音哥" "http://ojvx9eehr.bkt.clouddn.com/%E5%9B%9E%E9%9F%B3%E5%93%A5%20-%20%E4%B9%9E%E4%B8%90.mp3" %}

---


## 1. Ajax前导
### (1) XMLHttpRequest
*兼容： 除IE6
ie6 -> 报错

<!-- more -->

### (2) readyState就绪状态
- `0` uninitialized ajax  对象创建成功
- `1` loading  打开连接(已经建立连接)
- `2` loaded  发送数据
- `3` interactive  下载内容
- `4` complete  完成

### (3) HTTP状态码
**重点**
- `200` ok
- `304` not modified
- `403` Forbidden
- `404` Not found
- `405` Not allowed
- `414` Request-URI Too Long
- `500` Internal Server Error
- `502` Bad Gateway

> 附：全部状态码
'100': 'Continue',
'101': 'Switching Protocols',
'102': 'Processing',
'200': 'OK',
'201': 'Created',
'202': 'Accepted',
'203': 'Non-Authoritative Information',
'204': 'No Content',
'205': 'Reset Content',
'206': 'Partial Content',
'207': 'Multi-Status',
'208': 'Already Reported',
'226': 'IM Used',
'300': 'Multiple Choices',
'301': 'Moved Permanently',
'302': 'Found',
'303': 'See Other',
'304': 'Not Modified',
'305': 'Use Proxy',
'307': 'Temporary Redirect',
'308': 'Permanent Redirect',
'400': 'Bad Request',
'401': 'Unauthorized',
'402': 'Payment Required',
'403': 'Forbidden',
'404': 'Not Found',
'405': 'Method Not Allowed',
'406': 'Not Acceptable',
'407': 'Proxy Authentication Required',
'408': 'Request Timeout',
'409': 'Conflict',
'410': 'Gone',
'411': 'Length Required',
'412': 'Precondition Failed',
'413': 'Payload Too Large',
'414': 'URI Too Long',
'415': 'Unsupported Media Type',
'416': 'Range Not Satisfiable',
'417': 'Expectation Failed',
'418': 'I\'m a teapot',
'421': 'Misdirected Request',
'422': 'Unprocessable Entity',
'423': 'Locked',
'424': 'Failed Dependency',
'425': 'Unordered Collection',
'426': 'Upgrade Required',
'428': 'Precondition Required',
'429': 'Too Many Requests',
'431': 'Request Header Fields Too Large',
'451': 'Unavailable For Legal Reasons',
'500': 'Internal Server Error',
'501': 'Not Implemented',
'502': 'Bad Gateway',
'503': 'Service Unavailable',
'504': 'Gateway Timeout',
'505': 'HTTP Version Not Supported',
'506': 'Variant Also Negotiates',
'507': 'Insufficient Storage',
'508': 'Loop Detected',
'509': 'Bandwidth Limit Exceeded',
'510': 'Not Extended',
'511': 'Network Authentication Required'


### (4) ajax提交方式
- GET 数据在open提交
      载体：url

- POST 数据在send提交
	  载体：请求头(setRequestHeader)

## 2. Ajax

原生编写ajax.js

``` javascript
<script>
	function ajax(json) {
		// url, data, type, success, error, time, loading, complete
		//路径，数据，方式，成功回调函数，失败回调函数，超时时间，等待动画，完成后的回调函数
		// data数据格式为json

		json = json || {};
		if(!json.url) {
			alert('url is null!');
			return;
		}

		json.data = json.data || {};
		json.type = json.type || 'get'; // 默认提交方式为GET
		json.time = json.time || 3000;  // 默认超时时间为3000ms

		var timer = null;
		clearTimeout(timer);

		// 1. 获得ajax
		if(window.XMLHttpRequest) {  // 查看当前浏览器XMLHttpRequest是否是全局变量
			var oAjax = new XMLHttpRequest();
		} else {
			var oAjax = new ActiveXObject('Microsoft.XMLHTTP');  // IE6，传入微软参数
		}

		switch(json.type.toLowerCase()) {
			case 'get':
				// 2. 打开地址
				oAjax.open('GET', json.url + '?' + jsonToURL(json.data), true);  // 提交方式(大写)，url，是否异步
				// 3. 发送数据
				oAjax.send();
				break;
			case 'post':
				// 2. 打开地址
				oAjax.open('POST', json.url, true);  // 提交方式(大写)，url，是否异步
				oAjax.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');  // 设置请求头
				// 3. 发送数据
				oAjax.send(jsonToURL(json.data));

		}

		json.loading && json.loading();  // 执行等待动画

		// 4. 接收数据
		oAjax.onreadystatechange = function() {  // 监控状态
			if(oAjax.readyState == 4) {
				json.complete && json.complete();
				if(oAjax.status >= 200 && oAjax.status < 300 || oAjax.status == 304) {
					json.success && json.success(oAjax.responseText);  // 执行成功的回调函数，responseText为响应内容
				} else {
					json.error && json.error(oAjax.status);  // 执行失败的回调函数
				}
				clearTimeout(timer);
			}
		};

		// 网络超时时执行
		timer = null;
		timer = setTimeout(function() {
			console.log('请求超时');
			oAjax.onreadystatechange = null;
		}, json.time);

	}

	function jsonToUrl(json) {
		var arr = [];
		for(var name in json) {
			arr.push(name + '=' + json[name]);
		}
		return arr.join('&');
	}
</script>
```

## 3. Ajax服务器相关
- `oAjax.getAllResponseHeaders();`  获取ajax服务全部信息
- `oAjax.getResponseHeader('xxx');`  获取ajax服务器相关信息

## 4. Ajax2.0事件
- `oAjax.onload`  替代`oAjax.onreadystatechange`
- `oAjax.onerror`  发生错误(网络层级的错误才会触发)
- `oAjax.onprogress`  上传进度(ev.loaded/ev.total)
- `oAjax.onabort`  中断

## 5. 关于锚点hash

应用：刷新保留分页页码

> cookie只有4k左右，此处应用hash保留分页页码

``` css
<style>
	a{
	    display: block;
	    width: 50px;
	    height: 50px;
	    border:1px solid #000;
	    text-align: center;
	    line-height: 50px;
	    text-decoration: none;
	    float: left;
	    margin:10px;
	}
	a:hover{
	    background: #f60;
	    color: #fff;
	}
	a.active{
	    background: #f60;
	    color: #fff;
	}
</style>
```

``` html
	<a href="javascirpt:;" class="active">1</a>
	<a href="javascirpt:;">2</a>
	<a href="javascirpt:;">3</a>
	<a href="javascirpt:;">4</a>
	<a href="javascirpt:;">5</a>
```

``` javascript
<script>
	window.onload = function() {
		var aA = document.getElementsByTagName('a');
		var hash = window.location.hash;
		var index = hash.substring(1);
		if(hash) {
			tab(index-1);
		}

		for (var i = 0; i < aA.length; i++) {
			aA[i].index = i;
			aA[i].onclick = function() {
				var index = this.index;
				tab(index);
				window.location.hash = '#' + (this.index + 1);
			};
		}

		function tab(index) {
			for (var i = 0; i < aA.length; i++) {
				aA[i].className = '';
			}
			aA[index].className = 'active';
		}
	};
</script>
```

## 6. Ajax跨域
**同源策略：Ajax只能同域名下取数据**

### js跨域请求方式
- jsonp(json with padding)
- 修改document.domain跨子域
- window.name
- H5 window.postMessage(IE6 7不支持)
- CORS(跨域资源共享) 设置header: Access-Control-Allow-Origin
- nginx反向代理

[JavaScript跨域总结与解决办法](http://www.cnblogs.com/rainman/archive/2011/02/20/1959325.html)

## 7. jsonp前导

> **jsonp原理**
动态创建script标签，利用script:src可以跨域的属性跨域

> **HTML里面所有带src属性的标签都可以跨域，如iframe，img，script等。**

**不需要服务器环境**

[jsonp接口测试网址](www.asilu.com)

## 8. jsonp
原生编写jsonp.js

``` javascript
<script>

	function jsonp(json) {
		// url, data, cbName, success
		// 路径，参数，回调函数名字，回调函数

		json = json || {};
		if(!json.url) {
			return;
		}
		json.data = json.data || {};
		json.cbName = json.cbName || 'cb';  // 默认回调函数名字为cb，回调函数名一般为cb或callback

		var fnNmae = 'jsonp_' + Math.random();
		fnName = fnName.replace('.', '');  // 定义随机函数名，但名内不能包含.

		// 全局函数防止与外部函数jsonp()重名
		window[fnName] = function(json2) {
			json.success && json.success(json2);
			oHead.removeChild(oS);
		};

		json.data[json.cbName] = fnName;
		var arr = [];
		for(var name in json.data) {
			arr.push(name + '=' + json.data[name]);
		}

		var oS = document.createElement('script');  // **动态创建script标签
		var oHead = document.getElementsByTagName('head')[0];
		oS.src = json.url + '?' + arr.join('&');  // 动态script的src属性为jsonp的路径
		oHead.appendChild(oS);  // 插入到<head>标签最末
	}

</script>
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)