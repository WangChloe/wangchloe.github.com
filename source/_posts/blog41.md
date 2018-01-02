---
title: 2017-D2前端技术论坛 新思
date: 2018-01-02 03:27:35
categories: 前端札记
tags: [前端论坛]

---

- 天猫超市Mobile Web的极致体验优化
  - 回归web解决问题
  - web目前的不足
  - 解决方向

<!-- more -->

---

- [2017-D2](http://d2forum.alibaba-inc.com/#/index?_k=ux8vkk)

> **匠心** -> `工匠精神` `简单的事做好` `简单的事做的更好`


## 天猫超市Mobile Web的极致体验优化

- [天猫超市](https://chaoshi.m.tmall.com/)

### 回归web解决问题：
- 滚动过程代码冻结(Native解决)
    - WKWebview(ios8+) 替代 UIWebview组件
        - 在性能、稳定性、功能方面有很大提升
        （最直观的体现就是加载网页是占用的内存，模拟器加载百度与开源中国网站时，WKWebView占用23M，而UIWebView占用85M）
        - 允许JavaScript的Nitro引擎库加载并使用（UIWebView中限制）
        - 支持了更多的HTML5特性
        - 高达60fps的滚动刷新率以及内置手势
        - 将UIWebViewDelegate与UIWebView重构成了14类与3个协议
    - 浏览器内核U4
      - 极速引擎，采用全新的V8引擎，在U4 1.0的基础上，性能继续提高10%，重新打开网页的速度提高10%-20%；
      - 全新的渲染架构，新的Passive Event Listener、Intersection Observer等能力赋予H5页面更加流畅的操作体验；
      - 标准领先，新内核支持最新的PWA技术以及其他最新的H5、JS、CSS标准，并在国内首次提供标准Web推送服务；
      - 创新扩展，推出业界效率最高的Web AR技术，兼顾Native的体验和H5的高效率，还有其他更多的创新扩展值得期待。
- 加载慢
    - 预加载
- 页头无法定制
    - 隐藏native页头，使用h5页头

### web目前的不足
![web目前的不足](https://ws2.sinaimg.cn/large/006tKfTcgy1fmumfdhjmmj315k09sjv1.jpg)

### 解决方向

#### 精细化
- 还原度
- 字行高
- Android文字垂直居中
    - 非miui下都是偏上的
    ``` css
    .box{
        display:-webkit-box;
        display:flex;
        -webkit-box-pack:center; /*水平居中*/
        -webkit-box-align:center; /*垂直居中*/
        align-items:center; /*垂直居中*/
        justify-content:center; /*水平居中*/
        width:2rem;
        height:.3rem;
        margin:1rem;
        font-size:.24rem;
        overflow:hidden;
    }
    ```

#### 操作体验
- 极速响应 100ms内响应用户操作
    - 并行加载资源和数据 **Promise.all()**
    - 足够快时不需要loading
    ![足够快时不需要loading](https://ws2.sinaimg.cn/large/006tKfTcgy1fmvc87pt71j31660t6qfn.jpg)
- 实时反馈
    - 切换tabview时下边框线实时响应手势过程
- 操作流畅
    - 动画
      - 只使用**transform 3D/opacity**
      - 适时使用**will-change**  `will-change:transform; /*创建新的渲染层*/` 
        - [-webkit-transform:translate3d(0,0,0)触发GPU加速，让网页动画更流畅](http://blog.csdn.net/caicaijingyuan/article/details/44244991)
        - [使用CSS3 will-change提高页面滚动、动画等渲染性能](http://www.zhangxinxu.com/wordpress/2015/11/css3-will-change-improve-paint/)
        ![translate3d & will-change](https://ws4.sinaimg.cn/large/006tKfTcgy1fms39ippvyj31ia0cmtdi.jpg)
    - 滚动
      - 必须使用**passive event listeners**
        > 由于浏览器无法预先知道一个事件处理函数中会不会调用 preventDefault()，它需要等到事件处理函数执行完后，才能去执行默认行为，若监听完后再执行默认行为会导致页面卡顿。

        ![passive event listeners](https://ws3.sinaimg.cn/large/006tKfTcgy1fmundaoz61j3132072q65.jpg)
        - 我们可以通过传递 passive 为 true 来明确告诉浏览器，事件处理程序不会调用 preventDefault 来阻止默认滑动行为。
        - [移动Web滚动性能优化: Passive event listeners](https://segmentfault.com/a/1190000007913386?_ea=1507605)
        - Chrome 51 和 Firefox 49 已经支持 passive 属性
        ``` javascript
        var supportsPassive = false;
        try {
          var opts = Object.defineProperty({}, 'passive', {
            get: function() {
              supportsPassive = true;
            }
          });
          window.addEventListener("test", null, opts);
        } catch (e) {}

        elem.addEventListener(
          'touchstart',
          fn,
          supportsPassive ? { passive: true } : false
        ); 
        ```
    - 手势
      - 配合使用touchmove & scroll 事件
        > ios第一次触发scroll存在延时，应配合使用touchmove

#### 可靠性
- 预加载
    - service worker实现离线页面访问
    ![service worker push](https://ws3.sinaimg.cn/large/006tKfTcgy1fmuo9a47zpj312w0lsdjk.jpg)
    ``` javascript
    // 注册 service worker
    if ('serviceWorker' in navigator) {
        navigator.serviceWorker.register('./service-worker.js').then(function(registration) {
        // 注册成功
        console.log('ServiceWorker registration successful with scope: ', registration.scope);
    }).catch(function(err) {
        // 注册失败
        console.log('ServiceWorker registration failed: ', err);
       });
    }
    ```
    ``` javascript
    'use strict';

    var cacheVersion = 0;
    var currentCache = {
      offline: 'offline-cache' + cacheVersion
    };
    const offlineUrl = 'offline.html';

    // 首次缓存
    this.addEventListener('install', event => {
      event.waitUntil(
        caches.open(currentCache.offline).then(function(cache) {
          return cache.addAll([
              './offline.svg',
              offlineUrl
          ]);
        })
      );
    });

    // 拦截请求
    this.addEventListener('fetch', event => {

      if (event.request.mode === 'navigate' || (event.request.method === 'GET' && event.request.headers.get('accept').includes('text/html'))) {
            event.respondWith(
              fetch(event.request.url).catch(error => {
                  // Return the offline page
                  return caches.match(offlineUrl);
            })
         );
      }
      else{
            event.respondWith(caches.match(event.request)
                            .then(function (response) {
                            return response || fetch(event.request);
                        })
                );
          }
    });

    ```
    
    ![service workder兼容性](https://ws2.sinaimg.cn/large/006tKfTcly1fmvb3ae3whj31ae0g277h.jpg)
#### 设计语言APP化
- 无闪烁tabbar
![设计语言APP化](https://ws1.sinaimg.cn/large/006tKfTcgy1fmunohp8a1j316w0lg1kx.jpg)

> PS:同行者
- 业务线庞大
- 技术纬度不同

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！

公众号不发文的时候推送一些我觉得好用的前端网站或者看到的一些问题的解决方案，也更便于大家交流，就酱。

![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)