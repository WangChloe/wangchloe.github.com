---
title: vue小记
categories: 前端札记
date: 2019-03-24 14:39:21
tags: "vue"
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---


<!-- more -->

### v-html使用filter

`v-html="$options.filters.highlight(option.title)"`


### 销毁window.scroll
``` javascript
mounted(){
   window.addEventListener('scroll', this.scrollListener,false)
},
destroyed(){
    window.removeEventListener('scroll',this.scrollListener,false)
}

```

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)