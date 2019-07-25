---
title: NativeApi思考
date: 2019-07-23 03:27:35
categories: 前端札记
tags: [webview]

---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---



<!-- MarkdownTOC -->

- 以调用Native上传图片组件为例
- Hybrid通信原理
    - JS Bridge

<!-- /MarkdownTOC -->

<!-- more -->


### 以调用Native上传图片组件为例

```javascript
<script>
    NS.register("NativeApi");

    // 目前Android webview不支持type=file事件，通过协议解决：
    // 定义全局函数用于回调 
    window["nativeTakePhotoCallback"] = function(result, url, ud) {};

    //options参数若有相同项，则替换$.extend内的第三个{}类目
    NS.NativeApi.add("takePhoto", function(options) {
        var settings = $.extend(true, {}, {
            imgMaxWidth: 640,
            uploadUrl: "{0}xiaozu/ajaxUploadImg".format(currentApiRoot),
            userData: "",
            t: 0,
            callback: "nativeTakePhotoCallback"
        }, options);

        var protocol = "ns://{0}/dev/takephoto?cb={1}&w={2}&ud={3}&upload={4}&t={5}"
            .format(domain,
                rebuildCallback(settings.callback),
                settings.imgMaxWidth,
                settings.userData,
                // 把字符串作为 URI 组件进行编码，其中的某些字符将被十六进制的转义序列进行替换。
                // eg: : -> %3A   / -> %2F
                encodeURIComponent(settings.uploadUrl),
                settings.t);
        connectApp(protocol);
    });

    // js Bridge建立Native与H5间通信
    function connectApp(protocol) {
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


    function rebuildCallback(cb) {
        var t = cb;

        if ($.isFunction(cb)) {
            t = "hybrid_" + (+new Date());
            window[t] = function() {
                // 将具有length属性的对象转成数组
                cb.apply(null, Array.prototype.slice.call(arguments, 0));
                delete window[t];
            }
        }

        return t;
    }
</script>
```

---

- format传入参数实现

"//webapp/js/base.js",
```javascript
<script>
    String.prototype.format = function () {
        for (var temS = this, i = 0; i < arguments.length; ++i) {
            temS = temS.replace(new RegExp("\\{" + i + "\\}", "g"), arguments[i]);
        }
        return temS;
    }
</script>
```

- `+new Date()`

```javascript
<script>
    s = new Date().toString();
    // "Wed May 17 2017 11:00:16 GMT+0800 (中国标准时间)"

    t = (+new Date()).toString();
    // "1494990039861"
    // +new Date(); 等同于 new Date().getTime(); 简略写法，得到毫秒
</script>
```


- `Array.prototype.slice.call(arguments, 0)`剖析

将具有length属性的对象转成数组

```javascript
<script>
//ES5 中的数组方法slice的底层内部实现
Array.prototype.slice=function(start,end){
    var result = new Array(); //新数组
    var start = start || 0;
    var end = end || this.length; //this指向调用的对象，用了call之后，改变this的指向，指向传进来的对象
    for(var i=start; i<end; i++){
        result.push(this[i]);
    }
    return result;	//返回的为一个新的数组
}
</script>
```

```javascript
<script>
    // 传入arguments类数组，调用Array.prototype.slice原型方法
    // 并用call()方法，将作用域限定在arguments中
    // 这里Array.prototype就可以理解为arguments
    // 参数0为slice()方法的第一个参数，即开始位置索引，返回整个数组。
    Array.prototype.slice.call(arguments, 0);
</script>
```

- JS Bridge建立Native与H5间通信

```javascript
<script>
function connectApp(protocol) {
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


#### JS Bridge

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

[连续五篇讲述Hybrid以及JSBridge解决方案](http://www.cnblogs.com/dailc/p/5930231.html)



---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)