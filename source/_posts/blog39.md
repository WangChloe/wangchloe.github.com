---
title: 每天10个前端知识点：CSS3(2)
date: 2017-06-02 03:27:35
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

## 3. CSS3新增属性

### 色值透明度 rgba
之前的opacity改变背景色透明度时同时改变文字颜色透明度

rgba可实现**只改变背景色透明度**

### 文字阴影 text-shadow

text-shadow: [x轴 y轴 模糊度 弥散度 颜色]

- x轴：正值 -> 向右 负值 -> 向左

- y轴：正值 -> 向下 负值 -> 向上

阴影叠加

### 文字省略 text-overflow
``` css
<style>
    .ell {
        white-space: nowrap;  /* 不允许换行 */
        overflow: hidden;     /* 超出隐藏 */
        text-overflow: ellipsis;  /* 超出显示省略号，默认为clip(无省略号) */
    }
</style>
```

### 圆角 border-radius
`border-radius: 10px 20px 30px 40px` -> 左上角起顺时针经过的角的顺序

`border-radius: 10px 20px 30px 40px / 30px 10px 40px 20px`  **/** -> 分离x/y轴方向半径

### 盒子阴影 box-shadow
`box-shadow: [inset]  10px   20px   30px      40px   black;`

             [内阴影] x位移 y位移 模糊半径  弥散半径 颜色

多边框实例
``` css
<style>
    #box{
        width: 100px;
        height: 100px;
        box-shadow: 0 0 0 10px black,
                    0 0 0 20px green,
                    0 0 0 30px pink,
                    0 0 0 40px purple,
                    0 0 0 50px orange,
                    0 0 0 60px khaki,
                    0 0 0 70px indigo,
                    0 0 0 80px plum,
                    0 0 0 90px violet;
        margin: 200px auto;
    }
</style>
```

### 变换 transform (搭配transition使用效果更佳)

**以下属性可以一同使用**

- rotate([deg])  旋转

`transform: rotate(30deg);`  正向旋转30度
`tansform: rotate(0.785rad);`  正向旋转0.785弧度
`tansform: rotateX|Y|Z(45deg);`

> 角度转弧度 π/180×角度
弧度变角度 180/π×弧度

transofrm-origin 旋转中心

eg:
`transofrm-origin: left|top|bottom|right|center(默认);`
`transofrm-origin: left top|left bottom|right top|left center|center bottom;`
`transofrm-origin: -120px -120px;`

- translate(x, y)  偏移
x>0 右偏移
x<0 左偏移
y>0 下偏移
y<0 上偏移

`transform: translate(-30px, -40px);` 左偏移30px, 右偏移40px
`transform: translateX|Y|Z(-30px);`

- scale(s); 放大/缩小

`transform: scale(0.2)` 宽高缩小至0.2倍的大小
`transform: scale(0.2, 1)` 宽度缩小至0.2倍大小，高度不变
`transform: scaleX|scaleY(0.2)`

- skew([deg]) 倾斜

`transform: skew(20deg)` 水平向左倾斜20度
`transform: skewX(20deg)` 水平向左倾斜20度
`transform: skewY(20deg)` 垂直向上倾斜20度
`transform: skew(20deg, 20deg)` 水平向左倾斜20度，垂直向上倾斜20度

- 关于多属性

> **transform多属性时从后向前解析**

eg:
`transform: rotate(45deg) scale(2,1);`  先横向放大再旋转
`transform: scale(2,1) rotate(45deg);`  先旋转再横向放大

### 过渡 transition
transition 属性是一个简写属性，用于设置**四个过渡属性**：

- transition-property 设置过渡效果的 CSS 属性的名称

- transition-duration 完成过渡效果需要多少秒

- transition-timing-function 速度效果的速度曲线
    - linear      匀速等于 cubic-bezier(0,0,1,1)
    - ease        慢速开始，然后变快，然后慢速结束 cubic-bezier(0.25,0.1,0.25,1)
    - ease-in     慢速开始 等于 cubic-bezier(0.42,0,1,1)
    - ease-out    慢速结束 等于 cubic-bezier(0,0,0.58,1)
    - ease-in-out 慢速开始和结束 等于 cubic-bezier(0.42,0,0.58,1)
    - cubic-bezier(n,n,n,n)

- transition-delay 过渡效果何时开始

eg:

``` css
<style>
    #box{
        width: 200px;
        height: 200px;
        background-color: green;
        transition: 1s width cubic-bezier(1, 1.7, 0, 1.54) 1s;  /* 1s后由400px过渡到200px */
    }
    #box:active{
        width: 400px;
        transition: 0s; /* 按下0s后变为400px */
    }
</style>
```

### 动画 animation
animation 属性是一个简写属性，用于设置**六个动画属性**：

- animation-name 需要绑定到选择器的 keyframe 名称

- animation-duration 完成动画所花费的时间

- animation-timing-function 动画的速度曲线

- animation-delay 在动画开始之前的延迟

- animation-iteration-count 动画应该播放的次数
    - n        播放次数
    - infinite 无限次播放

- animation-direction 是否应该轮流反向播放动画
    - alternate 轮流反向播放
    - reverse   反向播放
    - alternate-reverse 反向交替播放

- animation-fill-mode 结束状态
    - forwards  停留在结束状态
    - backwards 返回原始状态

- animation-play-state 暂停动画
    - paused   动画已暂停
    - running 动画正在播放

keyframe 关键帧
``` css
<style>
    @keyframes name{
        from{

        }
        to{

        }

        10%{

        }
        20%{

        }
        100%{

        }
    }
</style>
```

### 视角 perspectiv
指定观察者与z=0平面的距离，使具有三维位置变换的元素产生透视效果。

[小tip: 纯CSS实现视差滚动效果](http://www.zhangxinxu.com/wordpress/2015/03/css-only-parallax-effect/)
[好吧，CSS3 3D transform变换，不过如此！](http://www.zhangxinxu.com/wordpress/2012/09/css3-3d-transform-perspective-animate-transition/)

perspective属性有两种书写形式：

- 用在舞台元素上（动画元素们的共同父辈元素）
  - `perspective:1px;` -> 给当前元素的子集增加视角

- 用在当前动画元素上，与transform的其他属性写在一起
  - `transform:perspective(800px) (perspective必须写在最前面) rotate...` -> 给当前元素增加视角


开启3D空间

`transform-style:preserve-3d;`  加给3D变化元素父集，不可继承

```
<style>
/*小tip: 纯CSS实现视差滚动效果*/
.container {
    /* 滚动容器 */
    perspective: 1px; 
    padding: 0; height: calc(100vh - 300px); overflow: auto;
}
.box {
    /* 视差元素的父级需要3D视角 */
    height: 1280px;
    transform-style: preserve-3d;
    position: relative;
}
.background {
    /* 滚动比较慢的背景元素 */
    position: absolute; left: 50%;
    transform: translate3D(-50%, -120px, -1px) scale(2);
}
</style>
```

### 文字转向 direction
需配合`unicode-bidi`使用

```
<style>
    /*right to left*/
    .rtl{
        direction:rtl;
        unicode-bidi:bidi-override;
    }
</style>
```

### 遮罩 -webkit-mask

```
<style>
.mask{
    -webkit-mask: url(xxx.jpg); /*背景按mask图片大小显示*/
    -webkit-mask:linear-gradient(rgba(0,0,0,1)), to(rgba(0,0,0,0); /*背景按渐变遮罩显示*/
}

</style>
```

[webkit下神奇的图层蒙版效果](http://wangchloe.vip/Demo/CSS3/mask)

![webkit下神奇的图层蒙版效果.gif](http://upload-images.jianshu.io/upload_images/2125655-57bad81f61dd6241.gif?imageMogr2/auto-orient/strip)

```
<style>

    body {
        margin: 0;
        padding: 0;
        background-color: #000;
    }
    #box {
        position: absolute;
        top:0;
        right:0;
        bottom:0;
        left:0;
        background: url(img/2.jpg) no-repeat;
        background-size: cover;
        -webkit-mask: -webkit-radial-gradient(300px 300px, circle, #f00 100px, rgba(0, 0, 0, .3) 20px);
    }

</style>
```
```
<div id="box"></div>
```
```
<script>

    var oBox = document.querySelector('#box');

    oBox.onmousemove = function({
        clientX,
        clientY
    }) {

        this.style.WebkitMask = `-webkit-radial-gradient(${
            clientX - oBox.offsetLeft
        }px ${
            clientY - oBox.offsetTop
        }px,circle,#f00 100px,rgba(0,0,0,.3) 20px)`

    }

</script>
```

### 倒影 box-reflect
[-webkit-box-reflect各个属性值效果演示](http://www.zhangxinxu.com/study/201608/-webkit-box-reflect.html)

box-reflect：none | <direction> <offset>? <mask-box-image>?

方向<direction> = above | below | left | right
间隔<offset> = <length> | <percentage>
遮罩<mask-box-image> = none | <url> | <linear-gradient> | <radial-gradient> | <repeating-linear-gradient> | <repeating-radial-gradient>

### 文字阴影 text-shadow
`text-shadow: x-shadow y-shadow [blur] [color];`

水平阴影位置 垂直阴影位置 模糊的距离 阴影颜色

### 调整尺寸 resize
是否允许用户调整元素的尺寸

```
resize: none;          用户无法调整元素的尺寸
resize: both;          用户可调整元素的高度和宽度
resize: horizontal;    用户可调整元素的宽度
resize: vertical;      用户可调整元素的高度
```

### background-image内的一些属性

#### linear-gradient 线性渐变

- direction:
to top    ->    0deg   -> 从下到上
to right  ->    90deg  -> 从左到右
to bottom ->    180deg -> 从上到下（默认值）
to left   ->    270deg -> 从右到左
to top left  -> 右上角到左上角（斜对角）
to top right -> 左下角到右上角（斜对角）

eg:
`background-image: linear-gradient(red,blue);`
`background-image: linear-gradient(250deg,red,blue);`
`background-image: linear-gradient(red,orange,yellow,green,blue,indigo,violet);`
`background-image: linear-gradient(red 50%,blue 50%);` 跳变
`background-image: linear-gradient(90deg,blue 33.33333%,white 33.33333%,white 66.66666%,red 66.66666%,red 100%);` 0~1/3是蓝色 1/3~2/3是白色 2/3~1是红色


#### radial-gradient 径向渐变

background:radial-gradient(position ,shape size, start-color, stop-color);

- position 圆心位置

- shape
circle  定义径向渐变为“圆形”
ellipse 定义径向渐变为“椭圆形”

- size
closet-side 指定径向渐变的半径长度为从圆心到离圆心最近的边
closest-corner  指定径向渐变的半径长度为从圆心到离圆心最近的角
farthest-side   指定径向渐变的半径长度为从圆心到离圆心最远的边
farthest-corner 指定径向渐变的半径长度为从圆心到离圆心最远的角

eg:
`background-image: radial-gradient(red,blue);` 内圆红色，外框蓝色
`background-image: radial-gradient(red,orange,yellow,green,blue,indigo,violet);border-radius: 50%;`
`background-image: radial-gradient(circle,red 30%,white 30%);`
`background-image: -webkit-radial-gradient(100px 100px,circle farthest-corner,red,blue);`
`background-image: -webkit-radial-gradient(100px 100px,circle farthest-side,red,blue);`
`background-image: -webkit-radial-gradient(150px 100px,200px 80px,red,blue);`

#### repeating-linear-gradient 重复渐变
eg:
`background-image: repeating-linear-gradient(red,blue 20%);`

### background相关属性

####background-origin 相对于内容框来定位背景图像

`background-origin: border-box;`  相对于边框盒来定位背景图像
`background-origin: content-box;` 相对于内容框来定位背景图像
`background-origin: padding-box;` 相对于内边距框来定位背景图像

#### background-clip 裁剪背景图片

`background-clip: border-box;`  相对于边框盒来定位背景图像
`background-clip: content-box;` 相对于内容框来定位背景图像
`background-clip: padding-box;` 相对于内边距框来定位背景图像

[webkit下神奇的文字蒙版效果](http://wangchloe.vip/Demo/CSS3/mask2)

![webkit下神奇的文字蒙版效果.PNG](http://upload-images.jianshu.io/upload_images/2125655-8fdaedb33502cb11.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
<style>

    #box {
        width: 200px;
        height: 200px;
        background: url(img/1.jpg);
        background-size: cover;
        -webkit-background-clip: text;
        font-size: 200px;
        font-weight: bolder;
        color: rgba(0, 0, 0, 0.1);
    }

</style>

```
```
    <div id="box">W</div>
```

#### background-attachment
`background-attachment: scroll` 默认值。背景图像会随着页面其余部分的滚动而移动。
`background-attachment: fixed`  当页面的其余部分滚动时，背景图像不会移动。

[图片覆盖切换效果](http://wangchloe.vip/Demo/CSS3/background-attachment)

![图片覆盖切换.gif](http://upload-images.jianshu.io/upload_images/2125655-16406daa0f269fae.gif?imageMogr2/auto-orient/strip)

#### background-size 背景图像尺寸
`background-size: 3em, 25%;`  自定义背景图像尺寸
`background-size: cover;`     使背景图像完全覆盖背景区域(**图像可能被裁剪**)
`background-size: contain;`   完全包含背景图(**no-repeat背景区域可能有空白**)

#### background-position 背景图像定位
常用于雪碧图

`background-position: center|top|right|bottom|left`
`background-position: 20px 20%`

### 滤镜 filter
[CSS3 Filter的十种特效](http://www.w3cplus.com/css3/ten-effects-with-css3-filter)

- blur 模糊
eg:
`filter: blur(20px);`

- drop-shadow 阴影
[CSS3 filter:drop-shadow滤镜与box-shadow区别应用](http://www.zhangxinxu.com/wordpress/2016/05/css3-filter-drop-shadow-vs-box-shadow/)
eg:
`filter: drop-shadow(100px 1px 2px black);`

- invert 反色
eg:
`filter: invert(0.7);`

- brightness 亮度
eg:
`filter: brightness(.5);`

- sepia 褐色
eg:
`filter: sepia(1);`

- hue-rotate 色相旋转
eg:
`filter: hue-rotate(30deg);`

- saturate 饱和度
eg:
`filter: saturate(10);`

## 4. CSS3媒体查询 media

### 媒体类型
- `all` 用于所有设备

- `screen` 用于电脑屏幕，平板电脑，智能手机等

- `print` 用于打印机和打印预览

### 媒体功能
- `width` 定义输出设备中的页面可见区域宽度

- `heigth` 定义输出设备中的页面可见区域高度

- `min-width` 定义输出设备中的页面最小可见区域宽度

- `max-width` 定义输出设备中的页面最大可见区域宽度

- `min-height` 定义输出设备中的页面最小可见区域高度

- `max-height` 定义输出设备中的页面最大可见区域高度

- `device-width` 定义输出设备的屏幕可见宽度

- `device-height` 定义输出设备的屏幕可见高度

- `orientation` 定义输出设备中的页面可见区域高度是否大于或等于宽度
  - `portrait` 竖屏 页面可见区域高度大于或等于宽度
  - `landscape` 横屏 页面可见区域高度小于宽度

### meida大全

``` css
<style>
    /* iphone3 */
    @media only screen and (min-device-width:320px) and (max-device-width:480px)  and (-webkit-device-pixel-ratio:1) {
    }
    @media only screen and (min-device-width:320px) and (max-device-width:480px)  and (-webkit-device-pixel-ratio:1) and (orientation:portrait) {
    }
    @media only screen and (min-device-width:320px) and (max-device-width:480px)  and (-webkit-device-pixel-ratio:1) and (orientation:landscape) {
    }

    /*iphone  4 (retina)*/
    @media only screen and (min-device-width:320px) and (max-device-width:480px)  and (-webkit-device-pixel-ratio:2) and (device-aspect-ratio:2/3) {
    }
    @media only screen and (min-device-width:320px) and (max-device-width:480px)  and (-webkit-device-pixel-ratio:2) and (device-aspect-ratio:2/3) and (orientation:portrait) {
    }
    @media only screen and (min-device-width:320px) and (max-device-width:480px)  and (-webkit-device-pixel-ratio:2) and (device-aspect-ratio:2/3) and (orientation:landscape) {
    }

    /*iphone 5*/
    @media only screen and (min-device-width:320px) and (max-device-width:568px)  and (-webkit-device-pixel-ratio:2) and (device-aspect-ratio:40/71) {
    }
    @media only screen and (min-device-width:320px) and (max-device-width:568px)  and (-webkit-device-pixel-ratio:2) and (device-aspect-ratio:40/71) and (orientation:portrait) {
    }
    @media only screen and (min-device-width:320px) and (max-device-width:568px)  and (-webkit-device-pixel-ratio:2) and (device-aspect-ratio:40/71) and (orientation:landscape) {
    }

    /*ipads (all) */
    @media only screen and (min-device-width:768px) and (max-device-width:1024px) {
    }
    @media only screen and (min-device-width:768px) and (max-device-width:1024px) and (orientation:portrait) {
    }
    @media only screen and (min-device-width:768px) and (max-device-width:1024px) and (orientation:landscape) {
    }

    /* ipad-retina */
    @media screen and (min-device-width:768px) and (max-device-width:1024px)  and (-webkit-device-pixel-ratio:2) {
    }
    @media screen and (min-device-width:768px) and (max-device-width:1024px)  and (-webkit-device-pixel-ratio:2) and (orientation:portrait) {
    }
    @media screen and (min-device-width:768px) and (max-device-width:1024px)  and (-webkit-device-pixel-ratio:2) and (orientation:landscape) {
    }

    /* iPhone 6 */
    /* Landscape */
    @media only screen 
    and (min-device-width:375px) /* or 213.4375em or 3in or 9cm */
    and (max-device-width:667px) /* or 41.6875em */
    and (width:667px)  /* or 41.6875em */
    and (height:375px)  /* or 23.4375em */
    and (orientation:landscape) 
    and (color:8)
    and (device-aspect-ratio:375/667)
    and (aspect-ratio:667/375)
    and (device-pixel-ratio:2)
    and (-webkit-min-device-pixel-ratio:2) {
    }

    /* Portrait */
    @media only screen 
    and (min-device-width:375px)  /* or 213.4375em */
    and (max-device-width:667px)  /* or 41.6875em */
    and (width:375px)  /* or 23.4375em */
    and (height:559px)  /* or 34.9375em */
    and (orientation:portrait) 
    and (color:8)
    and (device-aspect-ratio:375/667)
    and (aspect-ratio:375/559)
    and (device-pixel-ratio:2)
    and (-webkit-min-device-pixel-ratio:2) {
    }

    /* ----------- iPhone 6+ ----------- */

    /* Portrait and Landscape */
    @media only screen 
      and (min-device-width: 414px) 
      and (max-device-width: 736px) 
      and (-webkit-min-device-pixel-ratio: 3) { 

    }

    /* Portrait */
    @media only screen 
      and (min-device-width: 414px) 
      and (max-device-width: 736px) 
      and (-webkit-min-device-pixel-ratio: 3)
      and (orientation: portrait) { 
    }

    /* Landscape */
    @media only screen 
      and (min-device-width: 414px) 
      and (max-device-width: 736px) 
      and (-webkit-min-device-pixel-ratio: 3)
      and (orientation: landscape) { 

    }

</style>
```
[常见移动设备的 CSS3 Media Query 整理（iPhone/iPad/Galaxy/HTC One etc.）](https://segmentfault.com/a/1190000002711737)
[Media Queries for Standard Devices](https://css-tricks.com/snippets/css/media-queries-for-standard-devices/)

---

更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！

公众号不发文的时候推送一些我觉得好用的前端网站或者看到的一些问题的解决方案，也更便于大家交流，就酱。

![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)