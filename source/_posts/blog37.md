---
title: 每天10个前端知识点：杂技
date: 2017-05-19 03:27:35
categories: 前端札记
tags: [杂技]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

- toString\(\) && valueOf\(\)
- JSON stringify & parse
- +new Date\(\)
- Array.prototype.slice.call\(arguments, 0\) 剖析
- JS Bridge建立Native与H5间通信
    - Hybrid通信原理
    - JS Bridge

<!-- more -->

---

有些平时碰到的很零碎的东西我就随便插入到这个章节里了。

## 1. toString() && valueOf()

- toString() 把一个逻辑值转换为字符串，并返回结果。

- valueOf()  返回Boolean对象的原始值

> **源自知乎**
这两个方法一般是交由JS去隐式调用，以满足不同的运算情况。

> 在数值运算里，会优先调用valueOf()，如a+b；
在字符串运算里，会优先调用toString()，如alert(c)。

``` javascript
<script>
    // 该示例来源于脚本之家
    var bbb = {
        i: 10,
        toString: function() {
            console.log('toString');
            return this.i;
        },
        valueOf: function() {
            console.log('valueOf');
            return this.i;
        }
    }

    alert(bbb); // 10 toString
    alert(+bbb); // 10 valueOf
    alert('' + bbb); // 10 valueOf
    alert(String(bbb)); // 10 toString
    alert(Number(bbb)); // 10 valueOf
    alert(bbb == '10'); // true valueOf
    alert(bbb === '10'); // false
</script>
```

``` javascript
<script>
    console.log({
        valueOf: function() {
            return 20;
        }
    } * {
        valueOf: function() {
            return 30;
        }
    });     // 600
</script>
```

## 2. JSON stringify & parse

[json2.js - 引入解决IE7及以下版本JSON未定义问题。](https://github.com/douglascrockford/JSON-js/blob/master/json2.js)

1. JSON.stringify(object);  **对象 -> 字符串**  将对象字符串序列化成标准JSON字符串

eg: `{a:1,b:2}`  ->  `"{"a":1,"b":2}"`

2. JSON.parse(str);  **字符串 -> json对象**  将字符串序列化成对象

`{"name":"wangchloe","age":"22"}`  ->
```
{
    age: "22",
    name: "wangchloe",
    _proto: Object
}
```

``` html
<a href="https://www.baidu.com/" attr1='13'>baidu.com</a>
```

``` javascript
<script>
    var oA = document.querySelector('a');
    console.log(oA.getAttribute('attr1'));  // 13

    oA.setAttribute('attr1', '14');
    var num = oA.getAttribute('attr1');

    console.log(oA.getAttribute('attr1'));  // 14
    console.log(typeof number);  // string 直接设置自定义属性只能得到string类型

    oA.setAttribute('attr1', JSON.stringify({name: 14}));

    var num2 = oA.getAttribute('attr1');

    console.log(num2);  // {"name": "14"}
    console.log(JSON.parse(num2));
    // Object {name: "14"}
    //     name: "14"
    //     -> _proto_: Object

    console.log(JSON.parse(num2).name);  // 14
    console.log(typeof JSON.parse(num2).name);  // number JSON转化得到了真正类型
</script>
```

## 3. +new Date()
```
<script>
    s = new Date().toString();
    // "Wed May 17 2017 11:00:16 GMT+0800 (中国标准时间)"

    t = (+new Date()).toString();
    // "1494990039861"
    // +new Date(); 等同于 new Date().getTime(); 简略写法，得到毫秒
</script>
```

## 4. Array.prototype.slice.call(arguments, 0) 剖析

将具有length属性的对象转成数组

```
<script>
// array.js slice的内部实现
function slice(start, end) {
    var len = ToUint32(this.length),
        result = [];
    for (var i = start; i < end; i++) {
        result.push(this[i]);
    }
    return result;
}
</script>
```

```
<script>
    // 传入arguments类数组，调用Array.prototype.slice原型方法
    // 并用call()方法，将作用域限定在arguments中
    // 这里Array.prototype就可以理解为arguments
    // 参数0为slice()方法的第一个参数，即开始位置索引，返回整个数组。
    Array.prototype.slice.call(arguments, 0);
</script>
```

## 5. JS Bridge建立Native与H5间通信

### Hybrid通信原理
背景：原生APP开发中有一个webview的组件(Android中是webview,iOS7以下有UIWebview,7以上有WKWebview),这个组件可以加载Html文件。

- IOS
  - Object-C可直接调用js，只需调用stringByEvaluatingJavaScriptFromString即可，可直接获取js返回值。
  - js不可直接调用Object-C，利用 shouldStartLoadWithRequest，需拦截每个url，对指定的schema进行拦截做相应的本地方法。

- Android
  - Java可直接调用js，但不可直接获取js返回值。
  - Java注册addJavascriptInterface 后，js可直接调用Native的接口，并获取Native的返回值。[让Java跟Javascript更加亲密](http://www.alloyteam.com/2013/02/rang-java-gen-javascript-geng-jia-qin-mi/)
  - 通过 shouldOverrideUrlLoading，也还是拦截Web的所有URL请求来达到通信的目的。


**基础通信存在以下问题**

- Android4.2以下,addJavascriptInterface方式有安全漏洞

- iOS7以下,js无法调用Native

### JS Bridge

- url scheme交互方式是一套现有的成熟方案，可以完美兼容各种版本，不存在上述问题。

通过JSBridge(JS和Native通信机制),H5页面可以调用Native的api,Native也可调用H5页面的方法或者通知H5页面回调。

![JSBridge的核心原理](https://dailc.github.io/staticResource/blog/hybrid/jsbridge/img_hybrid_base_jsbridgePrinciple_1.png?_=5931322)

原理：
（1）初始化创建的一个style.display=none 的iframe,并将iframe.src设置为自有协议，每次js需要与Native通信时，js端主动调用iframe.src即可，Native收到请求通知后，反向调用fetchQueue(可见源码)获取消息内容；若Native需要与js通信，直接调用js，并获取返回值

（2）
- IOS
js->Native：js将要发送的消息存放在js端->调用iframe.src，触发通知Native->Native拦截请求，调用js bridge里面的fetchQueue并获取返回的消息内容，处理消息->将需要返回的数据通过直接调用js的方式，让js处理

- Android
js->Native: 通过 shouldOverrideUrlLoading 携带Js的返回值
（3）Native->js: Native可直接调用Js并获取返回的内容


```
<script>
function bridgeApp(protocol) {
    var iframe = document.createElement("iframe");
    var iframeStyle = document.createAttribute("style");
    var iframeSrc = document.createAttribute("src");

    iframeStyle.nodeValue = "display:none;width:0;height:0;";
    iframeSrc.nodeValue = protocol;
    iframe.setAttributeNode(iframeStyle);
    iframe.setAttributeNode(iframeSrc);
    document.body.appendChild(iframe);

    setTimeout(function() {
        document.body.removeChild(iframe);
    }, 250);
}
</script>
```


[连续五篇讲述Hybrid以及JSBridge解决方案](http://www.cnblogs.com/dailc/p/5930231.html)


---

更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！

公众号是坚持日更的，不发文的时候推送一些我觉得好用的前端网站或者看到的一些问题的解决方案，也更便于大家交流，就酱。

![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)