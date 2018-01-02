---
title: 每天10个前端知识点：js组成篇
date: 2017-01-18 03:27:35
categories: 前端札记
tags: [js]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

{% aplayer "My Prayer" "Devotion" "http://ojvx9eehr.bkt.clouddn.com/Devotion%20-%20My%20Prayer.mp3" %}


---
*本次内容有关js的三大组成。*


---

## 1. js实现的组成

### ECMA
ECMAScript(**js核心**) 提供核心语言功能

*兼容：完全兼容
eg:
` if(){}
arr.pop();`


<!-- more -->

### DOM
Document Object Model  文档对象模型  提供访问和操作网页内容的方法和接口

**DOM是针对XML但经过扩展用于HTML的应用程序接口(API)。**

*兼容：大部分兼容，不兼容可以作兼容处理
eg:
`document.getElementById('id')
 oDiv.style.background = 'red';`
### BOM
Browser Object Model  浏览器对象模型  提供与浏览器交互的方法和接口

*兼容：根据使用浏览器来，根本不兼容，且不能作兼容处理
eg:
`window.navigator.userAgent
 alert();`

## 2. DOM节点关系
1. 父子节点

- 子节点：父节点.children;	(一级，一层)

- 父节点：子节点.parentNode;

2. 兄弟节点

 - 上一个兄弟节点：
      obj.perviousElementSibling;
            *兼容：高级浏览器
      obj.previousSibling
            *兼容：全兼容
            高级浏览器 -> object text
            低级浏览器 -> 能获取相应的节点

	**兼容写法**

	var oPrev = obj.previousElementSibling || obj.previousSibling;
 - 下一个兄弟节点
       obj.nextElementSibling
             *兼容：高级浏览器
      obj.nextSibling
             *兼容：全兼容
              高级浏览器 -> object text
              低级浏览器 -> 能获取相应的节点

	**兼容写法**

	var oNext = obj.nextElementSibling || obj.nextSibling;

3. 首尾节点

    - 首节点
        父节点.firstElementChild

				*兼容：高级浏览器
				低级浏览器 -> undefined

	 父节点.firstChild

		         *兼容：全兼容
                  高级浏览器 -> object text
                  低级浏览器 -> 能获取相应的节点

	**兼容写法**

	(1) 父节点.fisrtElementChild || 父节点.firstChild

	(2) 父节点.children[0]

    - 尾节点
         父节点.lastElementChild
                    *兼容：高级浏览器
                     低级浏览器 -> undefined
         父节点.lastChild
                    *兼容：全兼容
                     高级浏览器 -> object text
                     低级浏览器 -> 能获取相应的节点

	**兼容写法**

	(1) 父节点.lastElementChild || 父节点.lastChild

	(2) 父节点.children[父节点.children.length - 1]

## 3. DOM节点操作
1. 创建一个节点
	var obj = document.createElement('tagName');
2. 添加一个节点
	父节点.appendChild(要添加的节点);
	父节点.insertBefore(要添加的节点, 在谁前面添加);
3. 删除一个节点
	父节点.removeChild(要删除的节点);
4. 替换一个节点
	父节点.replaceChild(新节点, 删除的节点);

## 4. DOM属性操作
1. .
2. []
3. 可操作自定义属性
  - 获取属性 obj.getAttribute(属性的名字);
  - 设置属性 obj.setAttribute(属性的名字, 值);
  - 删除属性 obj.removeAttribute(属性的名字);

> 获取设置属性方法尽量不混用

## 5. BOM
1. window.open(地址, 方式);	 打开新窗口
	返回值：新的窗体对象
		Chrome：拦截
		FF：阻止
		IE：直接打开
		*：用户自己打开的都不拦截
	打开方式：
		(1) _blank 新窗口打开(默认)
		(2) _self  当前页面打开
	about:blank  空白页
2. window.close();			关闭当前窗口
		Chrome：直接关闭
		FF：没有反应
		IE：提示
		*：只能关闭自己open出来的窗口
3. window.location  		获取地址栏信息
	返回值数据类型：object
  - window.location.href    	获取地址栏信息
	返回值数据类型：string
  - window.location.search  	获取地址栏信息中的数据
	返回值：?(包括?)后面的值
  - window.location.hash    	获取地址栏信息中的锚点
	返回值：#(包括#)后面的值
  - window.location.protocol 	获取地址栏信息中的协议
	返回值：eg: http:
  - window.location.host      获取地址栏信息中的域名
	返回值：eg：localhost:8080  baidu.com
  - window.location.port      获取地址栏信息中的端口
	返回值：eg：8080
   - window.location.pathname  获取地址栏信息中的路径
	返回值：eg：/../../xxx.html
4. window.history           获取地址的历史信息
 - window.history.forward()  前进
  - window.history.back()     后退
  - window.history.go(数字)	
  前进时数字>0  ->  1代表前进1个页面
  后退时数字<0  ->  -1代表后退1个页面
5. window.location.reload();  强制刷新页面



---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)