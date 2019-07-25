---
title: css合辑
categories: 前端札记
tags: ['css']
date: 2019-01-10 16:33:14
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

🐳专注css一百年，国际领先产品！

<!-- MarkdownTOC -->

- 弹性布局
- 水平垂直居中
- 三角形
- 加号
- 限制行数 段落超出...
- loading
- 横向溢出滚动
- 透明边框
- ul横排滚动
- 渐变文字
- 渐变边框
- input placeholder颜色

<!-- /MarkdownTOC -->


<!-- more -->

### 弹性布局

``` css
.fui-grid{display:-webkit-box; -webkit-box-sizing:border-box; box-sizing:border-box;}
.fui-span0{display:block; -webkit-box-flex:0;}
.fui-span1{display:block; -webkit-box-flex:1;}
.fui-span2{display:block; -webkit-box-flex:2;}
.fui-span3{display:block; -webkit-box-flex:3;}
```

### 水平垂直居中

``` css
.fui-uti-ver-mid { display: -webkit-box; -webkit-box-orient: vertical; -webkit-box-pack: center; -webkit-box-align: center; }
.fui-uti-hor-mid { display: -webkit-box; -webkit-box-orient: horizontal; -webkit-box-pack: center; -webkit-box-align: center; }
```

### 三角形

``` css
.fui-uti-arrow { display: inline-block; width: 8px; height: 8px; border: 1px solid #666; border-bottom: none; border-left: none; font-style: normal; -webkit-transform: rotate(135deg); transform: rotate(135deg); -webkit-transition: transform .3s ease-in-out; transition: transform .3s ease-in-out; }
.fui-uti-arrow-up { -webkit-transform: rotate(-45deg); transform: rotate(-45deg); }
.fui-uti-arrow-left { -webkit-transform: rotate(225deg); transform: rotate(225deg); }
.fui-uti-arrow-right { -webkit-transform: rotate(45deg); transform: rotate(45deg); }

```

### 加号
``` css
.fui-uti-plus { position: relative; display: inline-block; width: 0.3rem; height: 0.3rem; vertical-align: middle; }
.fui-uti-plus:before { position: absolute; top: 0.14rem; left: 0; width: 0.3rem; height: 0; border-top: 0.02rem solid #fff; content: ""; }
.fui-uti-plus:after { position: absolute; top: 0; left: 0.14rem; width: 0; height: 0.3rem; border-left: 0.02rem solid #fff; content: ""; }
```

### 限制行数 段落超出...
``` css
.fui-uti-nowrap-line { display: -webkit-box; overflow: hidden; text-overflow: ellipsis; -webkit-box-orient: vertical; -webkit-line-clamp: 1; }
.fui-uti-nowrap-two-line { display: -webkit-box; overflow: hidden; text-overflow: ellipsis; -webkit-box-orient: vertical; -webkit-line-clamp: 2; }
```

### loading

![](http://static2.51fanli.net/webapp/images/icons/loading.png)![](http://ww3.sinaimg.cn/large/006tNc79gy1g5bx0sg5m7j300t00tmwx.jpg)

``` css
/*loading*/
@-webkit-keyframes fui-loading-rotate {
	0% { -webkit-transform: rotate(0deg); }
	100% { -webkit-transform: rotate(360deg); }
}
@keyframes fui-loading-rotate {
	0% { transform: rotate(0deg); }
	100% { transform: rotate(360deg); }
}
.fui-uti-loading { display: -webkit-box; display: flex; -webkit-box-pack: center; -webkit-box-align: center; justify-content: center; align-items: center; margin: 0.1rem 0; color: #999; }
.fui-uti-loading:before { content: ''; width: 0.29rem; height: 0.29rem; margin-right: 0.08rem; background: url(/webapp/images/icons/loading.png) no-repeat; -webkit-background-size: 100% auto; background-size: 100% auto; -webkit-transform-origin: center; -webkit-animation: fui-loading-rotate 1s linear infinite; animation: fui-loading-rotate 1s linear infinite; }
```

### 横向溢出滚动
``` css
div {
    position: relative;
    white-space: nowrap;
    overflow-x: scroll;
    overflow-y: hidden;
    -webkit-overflow-scrolling: touch;
    overflow-scrolling: touch;
}
```

### 透明边框
``` css
.b {
    background: #fff; 
    border: .05rem solid rgba(255, 255, 255, .6);
    background-clip: padding-box; // 重要
}
```

### ul横排滚动

``` html
<style>
<!--第一种方案  不推荐！！！  低版本安卓格式错乱 -->
.wrap-of-scroll{overflow:hidden;margin-bottom:.2rem;}
.wrap-of-scroll ul{display:-webkit-box;display:flex;overflow-x:scroll;overflow-y:hidden;-webkit-overflow-scrolling:touch;overflow-scrolling:touch}
.wrap-of-scroll li{float:left;margin-right:.1rem;width:3rem;}

<!--第二种方案-->
.wrap-of-scroll{}
.wrap-of-scroll ul{overflow-x:scroll;overflow-y:hidden;-webkit-overflow-scrolling:touch;overflow-scrolling:touch;width:100%;white-space:nowrap;}
.wrap-of-scroll li{display:inline-block;margin-right:.1rem;width:3rem;}

</style>

<div class="wrap-of-scroll">
    <ul>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
    </ul>
</div>
```

### 渐变文字

``` css
.word{
    background:linear-gradient(to right, #000, #fff);
    -webkit-background-clip:text;
    -webkit-text-fill-color:transparent;
}
```

### 渐变边框

``` css
.banner-countdown{background:linear-gradient(to bottom,#f8c971, #e3a946);-webkit-background-clip:text;-webkit-text-fill-color:transparent;}

.banner-countdown em{border:2px solid;border-image: -webkit-linear-gradient(#f8c971, #e3a946) 3 3;-webkit-text-fill-color:initial;}
```

### input placeholder颜色
``` css
input::-webkit-input-placeholder{color:#ccc}

input::-webkit-input-placeholder, textarea::-webkit-input-placeholder{ color: #999; }
```

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)