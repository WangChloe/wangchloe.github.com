---
title: 每天10个前端知识点：CSS3(1)
date: 2017-05-19 03:27:35
categories: 前端札记
tags: [CSS3]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

- CSS3老版浏览器兼容处理
- CSS3新增选择器
    - 属性选择器
    - 结构选择器
- CSS3新增属性
    - 色值透明度 rgba
    - 文字阴影 text-shadow
    - 文字省略 text-overflow
    - 圆角 border-radius
    - 盒子阴影 box-shadow
    - 变换 transform \(搭配transition使用效果更佳\)
    - 过渡 transition
    - 动画 animation
    - 视角 perspectiv
    - 文字转向 direction
    - 遮罩 -webkit-mask
    - 倒影 box-reflect
    - 文字阴影 text-shadow
    - 调整尺寸 resize
    - background-image内的一些属性
    - background相关属性
    - 滤镜 filter
- CSS3媒体查询 media
    - 媒体类型
    - 媒体功能
    - meida大全


<!-- more -->

---


## 1. CSS3老版浏览器兼容处理

- css
  - -webkit-transition: 1s;
  - -moz-transition: 1s;
  - -o-transition: 1s;
  - transition: 1s;

- js(去掉-，首字母大写)
  - WebkitTransition
  - MozTransition
  - OTransition

## 2. CSS3新增选择器

### 属性选择器
**兼容：IE7+**

- E[attr]

- E[attr="value"]  匹配属性  **只含一个属性值**

- E[attr~="value"]  匹配空格隔开的单词  attr属性值包含value

- E[attr^="value"]  匹配开头  attr属性值以value开头

- E[attr$="value"]  匹配结尾  attr属性值以value结束

- E[attr*="value"]   attr属性值包含value字符

- E[attr|="value"]  匹配value-开头  attr属性值是value或者"value-"开头  **只含一个属性值**


``` css
<style>
    p[attr1] {
        background: #f00;
    }
    p[attr1=a] {
        background: #0ff;
    }
    p[attr1~=c] {
        background: #00f;
    }
    p[attr1^=e] {
        background: #ff0;
    }
    p[attr1$=g] {
        background: #f0f;
    }
    p[attr1*=i] {
        background: #0f0;
    }
    p[attr1|=k] {
        background: #fff;
    }
</style>
```

``` html
    <strong>E[attr="value"] **只含一个属性值**</strong>
    <p attr1="a">attr1="a"</p>
    <p attr1="b a">attr1="b a"</p>
    <hr/>
    <strong>E[attr~="value"] attr属性值包含value</strong>
    <p attr1="c">attr1="c"</p>
    <p attr1="d c">attr1="d c"</p>
    <hr/>
    <strong>E[attr^="value"] attr属性值以value开头</strong>
    <p attr1="e">attr1="e"</p>
    <p attr1="f ef">attr1="f ef"</p>
    <p attr1="ef f">attr1="ef f"</p>
    <hr/>
    <strong>E[attr$="value"] attr属性值以value结束</strong>
    <p attr1="g gh">attr1="g gh"</p>
    <p attr1="h hg">attr1="h hg"</p>
    <hr/>
    <strong>E[attr*="value"] attr属性值包含value字符</strong>
    <p attr1="i">attr1="i"</p>
    <p attr1="j jij">attr1="j jij"</p>
    <hr/>
    <strong>E[attr|="value"] attr属性值是value或者"value-"开头  **只含一个属性值**</strong>
    <p attr1="k">attr1="k"</p>
    <p attr1="l k">attr1="l k"</p>
    <p attr1="m k-m">attr1="m k-m"</p>
    <p attr1="k-m">attr1="k-m"</p>
```

![css3属性选择器](http://img.blog.csdn.net/20170110165816536?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdzk1MDkxNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 结构选择器
**兼容：IE9+**

**下标索引从1开始**

- `:nth-child(n) ` 找当前节点的父节点下的第n个子节点，**当且仅当该子节点与当前节点的类型相符时有效**

- `:nth-last-child(n)`  找当前节点的父节点下的倒数第n个子节点，**当且仅当该子节点与当前节点的类型相符时有效**

- `:nth-of-type(n)` 找当前节点的父节点下的第n个与当前节点的类型相符的子节点

- `:nth-last-of-type(n)` 找当前节点的父节点下的倒数第n个与当前节点的类型相符的子节点

- `:first-child`  即 :nth-child(1)

- `:last-child`  即 :nth-last-child(1)

- `:first-of-type`  即 :nth-of-type(1)

- `:last-of-type`  即 :nth-last-of-type(1)

- `:only-child`   当前节点的父节点下只有一个子节点时有效

- `:only-of-type` 当前节点的父节点下与当前节点的类型相符的子节点只有一个时有效，可有多个其他类型子节点

---

- `:target`  设置跳转锚点属性  <a href="#div1"></a> -> <div id="div1"></div>

- `:disabled`  不可点击的表单控件

- `:enabled`   可点击的表单控件

- `:checked`   选中的checkbox/radio

- `A~B`        A元素后的第一个类型为B的兄弟节点  **应用：模拟checkbox**

- `:first-line` 当前节点的第一行

- `:first-letter` 当前节点的第一个字符

- `:not(x)`      不匹配x时有效

---

伪元素

- `::selection`  在当前节点内选中文字时有效

- `::before`     搭配content使用，在当前节点内容最前面插入content **不增加DOM**

- `::after`     搭配content使用，在当前节点内容最后面插入content **不增加DOM**

---

更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！

公众号是坚持日更的，不发文的时候推送一些我觉得好用的前端网站或者看到的一些问题的解决方案，也更便于大家交流，就酱。

![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)