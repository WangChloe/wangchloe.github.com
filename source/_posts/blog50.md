---
title: webview回退
categories: 前端札记
tags: [webview]
date: 2019-05-24 14:47:24
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

<!-- MarkdownTOC -->

- webview切换回退监听
- webview指定回退至某页面

<!-- /MarkdownTOC -->


<!-- more -->

### webview切换回退监听

``` javascript
appSubsribe: function() {
    var that = this;

    document.addEventListener("visibilitychange", function(){
        if(!document.hidden) {
            that.fetchData();
        }
    });
}
```

### webview指定回退至某页面

``` javascript
function bindNativeBack() {
    if (!history.state) {
        history.pushState({}, null, null);
    }

    //超返首页回退到返利首页
    // 执行closewv?target协议，target链接中有‘？’，需encodeURIComponent转义
    window.addEventListener("popstate", function(ev) {
        Fanli.Utility.bridgeApp("ifanli://m.51fanli.com/app/action/closewv?target="+encodeURIComponent("ifanli://m.51fanli.com/app/show/native?name=sfmain"));
    });
}
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)