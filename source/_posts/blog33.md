---
title: 每天10个前端知识点：HTML5(线程、websocket)
date: 2017-04-04 03:27:35
categories: 前端札记
tags: [HTML5]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

- H5 web工作线程 webworker
    - \(1\) 方法
    - \(2\) 示例
- H5 webSocket 网络套接字

<!-- more -->

---

## 11. H5 web工作线程 webworker
- 进程
- 线程

`var worker = new Worker('js文件');`  新建worker

>   1. 需在服务器环境下
    2. 不会改变数据类型
    3. 不会改变父线程数据
    4. DOM/BOM 不可使用(console.log可用)
    5. 只能有一层子线程，子线程不可再开子线程

### (1) 方法
- worker.postMessage('Data')  向worker内传递数据 (1)
- worker.onmessage 监听事件 (4)
- worker.terminate 停止worker

worker内部
监听事件：
- this.onmessage -> ev -> ev.data //'Data' (2)
- this.postMessage 向父线程传递数据  (3)

### (2) 示例

- 主程序

``` javascript
<script>
    var worker = new Worker('./calc.js');

    worker.postMessage(2);
</script>
```

- calc.js

``` javascript
<script>
    this.onmessage = function(ev) {
        console.log(ev.data)
    }

    function fibonacci(n){
        if( n == 1 || n == 2 ) return 1;

        return fibonacci(n-1) + fibonacci(n-2);
    }
</script>
```

## 12. H5 webSocket 网络套接字

- 客户端
    1.发消息 -> emit
    2.接消息 -> on

- 服务端
    1.接消息 -> on
    2.发消息 -> emit

在以前 HTTP 协议中所谓的 keep-alive connection 是指在一次 TCP 连接中完成多个 HTTP 请求，但是对每个请求仍然要单独发 header。

WebSocket 解决的第一个问题是，通过第一个 HTTP request 建立了 TCP 连接之后，之后的交换数据都不需要再发 HTTP request了，使得这个长连接变成了一个真.长连接。

> Websocket只需要一次HTTP握手，所以说整个通讯过程是建立在一次连接/状态中，也就避免了HTTP的非状态性，服务端会一直知道你的信息，直到你关闭请求，这样就解决了接线员要反复解析HTTP协议。

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！

公众号是坚持日更的，不发文的时候推送一些我觉得好用的前端网站或者看到的一些问题的解决方案，也更便于大家交流，就酱。

![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)