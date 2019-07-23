---
title: 每天10个前端知识点：布局大全
date: 2017-02-20 03:27:35
categories: 前端札记
tags: [css]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。



---

本次内容主要整理来源：[布局 - 代码库 - NEC : 更好的CSS样式解决方案](http://nec.netease.com/library/category/#grid)

[以下代码可复制至codePen实践](http://codepen.io/pen/)

## 一. 单列布局

### 1. 水平居中

#### (1) 文本、图片等行内元素

```
<style>
	.parent {
		text-align: center;
	}
</style>
```

<!-- more -->

#### (2) 定宽块级元素

```
<style>
	.child {
		margin: 0 auto;
	}
</style>
```

#### (3) 不定宽块元素

- inline + text-align

```
<style>
	.parent {
		text-align: center;
	}
	.child {
		display: inline;
	}
</style>
```

- table + margin

```
<style>
	.child {
		display: table;
		margin: 0 auto;
	}
</style>
```

- float + relative

```
<style>
	.parent {
		float: left;
		position: relative;
		left: 50%;
	}
	.child {
		position: relative;
		left: -50%;
	}
</style>
```

- absolute + transform

```
<style>
	.parent {
		position: relative;
	}
	.child {
		position: absolute;
		left: 50%;
		transform: translateX(-50%);
	}
</style>
```

#### (4) 多个块级元素(单个块级元素也可用)

- inline-block + text-align

```
<style>
	.parent {
		text-align: center;
	}
	.child {
		display: inline-block;
		*display: inline;
		*zoom:1;
	}
</style>
```

- flex + justify-content

```
<style>
	.parent {
		display: flex;
	}
	.child {
		justify-content: center;
	}
</style>
```

### 2. 垂直居中

#### (1) 父元素高度确定的单行文本

```
<style>
	.parent {
		height: 20px;
		line-height: 20px;
		overflow:hidden;
	}
</style>
```

#### (2) 父元素高度确定的多行文本

```
<style>
	.parent {
		display: table;
	}
	.child {
		display: table-cell;
		vertical-align: middle;
	}
</style>
```

#### (3) 子元素定高块级元素

```
<style>
	.child {
		position: absolute;
		top: 50%;
		margin-top: -50px;
		width: 100px;
		height: 100px;
	}
</style>
```

#### (4) 子元素不定高块级元素

- absolute + transform

```
<style>
	.parent {
		position: relative;
	}
	.child {
		position: absolute;
		top: 50%;
		transform: translateY(-50%);
	}
</style>
```

- flex + align-items

```
<style>
	.parent {
		display: flex;
		align-items: center;
	}
</style>
```

### 3. 水平垂直居中

#### (1) 定宽高

```
<style>
	.child {
		position: absolute;
		top: 50%;
		left: 50%;
		margin-top: -75px;
		margin-left: -75px;
		width: 150px;
		height: 150px;
	}
</style>
```

#### (2) 不定宽高

- inline-block + table-cell

```
<style>
	.parent {
		display: tabel-cell;
		text-align: center;
		vertical-align: middle;
	}
	.child {
		display: inline-block;
		*display: inline;
		*zoom:1;
	}
</style>
```

- table-cell + absolute

```
<style>
	.parent {
		display: tabel;
		position: absolute;
		width: 100%;
		height: 100%;
	}
	.child {
		display: table-cell;
		text-align: center;
		vertical-align: middle;
	}
</style>
```

- absolute + transform

```
<style>
	.parent {
		position: relative
	}
	.child {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
	}
</style>
```

- flex + justify-content + align-items

```
<style>
	.parent {
		display: flex;
		justify-content: center;
		align-items: center;
	}
</style>
```

- jQuery

```
<script>
	$(window).resize(function() {
		$('.child').css({
			position: 'absolute',
			left: ($(window).width()-$('.child').outerWidth())/2,
			top: ($(window).height()-$('.child').outerHeight())/2
		});
	});

	$(function() {
		$(window).resize();
	})
</script>
```


## 二. 两列布局

### 1. 两个div并排的多种方法
- 定位
- margin负值
- 浮动
- 行内块
- table

### 2. 左侧定宽，右侧自适应

- float + margin

``` css
<style>
	.left {
		float: left;
		width: 100px;
	}
	.right {
		margin-left: 120px;
	}
</style>
```

``` html
	<div class="left">左侧定宽</div>
    <div class="right">右侧自适应</div>
```

- float + margin

``` css
<style>
	.left {
		float: left;
		width: 100px;
		margin-right: 20px;
	}
	.right {
		overflow: hidden;
	}
</style>
```

``` html
	<div class="left">左侧定宽</div>
    <div class="right">右侧自适应</div>
```

- absolute + margin

``` css
<style>
	.left {
		width: 300px;
		height: 500px;
		background: red;
		font-size: 30px;
		color: #fff;
		position: absolute;
		left: 0;
		top: 0;
	}
	.right {
		height: 500px;
		background: blue;
		font-size: 30px;
		color: #fff;
		/* padding-left: 300px; */
		margin-left: 300px;
	}
</style>
```

``` html
	<div class="left">左侧定宽</div>
    <div class="right">右侧自适应</div>
```

- relative + float + margin

``` css
<style>
	.g-sd1 {
		position: relative;
		float: left;
		margin-right: -190px;
		width: 190px;
	}
	.g-mn1 {
		float: right;
		width: 100%;
	}
	.g-mn1c {
		margin-left: 200px;
	}
</style>
```

``` html
    <div class="g-sd1">
        <p>左侧定宽</p>
    </div>
    <div class="g-mn1">
        <div class="g-mn1c">
            <p>右侧自适应</p>
        </div>
    </div>
```

- absolute + margin

``` css
<style>
	.parent {
		display: flex;
	}
	.left {
		margin-right: 20px;
		width: 100px;
	}
	.right {
		flex: 1;
	}
</style>
```

``` html
	<div class="parent">
		<div class="left">
			<p>左侧定宽</p>
		</div>
		<div class="right">
			<p>右侧自适应</p>
		</div>
	</div>
```

### 3. 左侧自适应，右侧定宽(基本同理，不一一列举)

- margin + absolute

``` css
<style>
	.left {
		/* padding-right: 300px; */
		margin-left: 300px;
		height: 500px;
		background: blue;
		font-size: 30px;
		color: #fff;
	}
	.right {
		position: absolute;
		right: 0;
		top: 0;
		width: 300px;
		height: 500px;
		background: red;
		font-size: 30px;
		color: #fff;
	}
</style>
```

``` html
	<div class="left">左侧自适应</div>
    <div class="right">右侧定宽</div>
```

- relative + float + margin

``` css
<style>
	.g-sd2 {
		position: relative;
		float: right;
		width: 230px;
		margin-left: -230px
	}
	.g-mn2 {
		float: left;
		width: 100%;
	}
	.g-mn2c {
		margin-right: 240px;
	}
</style>
```

``` html
    <div class="g-mn2">
        <div class="g-mn2c">
            <p>左侧自适应</p>
        </div>
    </div>
    <div class="g-sd2">
        <p>右侧定宽</p>
    </div>
```

## 三. 等高布局

- float + margin-bottom负值

``` css
<style>
	.box {
		width: 800px;
		overflow: hidden;	/* 搭配使用 */
		margin: 10px auto;
	}
	.l-box {
		float: left;
		margin-bottom: -2000px;		/* 搭配使用 */
		padding-bottom: 2000px;		/* 搭配使用 */
		width: 300px;
		background: red;
	}
	.r-box {
		float: left;
		margin-bottom: -2000px;		/* 搭配使用 */
		padding-bottom: 2000px;		/* 搭配使用 */
		width: 500px;
		background: blue;
	}
</style>
```

``` html
	<div class="box">
    	<div class="l-box">
			左侧左侧左侧左侧<br/>
			左侧左侧左侧左侧<br/>
			左侧左侧左侧左侧<br/>
			左侧左侧左侧左侧<br/>
    	</div>
        <div class="r-box">
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        </div>
    </div>
```

- flex

``` css
<style>
	.box {
		display: flex;
	}
	.l-box {
		margin-right: 20px;
		width: 100px;
	}
	.r-box {
		flex: 1;
	}
</style>
```

``` html
	<div class="box">
    	<div class="l-box">
			左侧左侧左侧左侧<br/>
			左侧左侧左侧左侧<br/>
			左侧左侧左侧左侧<br/>
			左侧左侧左侧左侧<br/>
    	</div>
        <div class="r-box">
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        	右侧右侧右侧右侧<br/>
        </div>
    </div>
```

## 四. 多列布局
### 1. 左右定宽，中间自适应

- absolute + margin

``` css
<style>
	.left {
		position: absolute;
		top: 0;
		left: 0;
		width: 200px;
		height: 500px;
		background: red;
		color: #fff;
		font-size: 20px;
	}
	.right {
		position: absolute;
		top: 0;
		right: 0;
		width: 200px;
		height: 500px;
		background: red;
		color: #fff;
		font-size: 20px;
	}
	.content {
		/* padding: 0 200px; */
		margin: 0 200px;
		height: 500px;
		background: blue;
		font-size: 20px;
		color: #fff;
	}
</style>
```

``` html
	<div class="left">左侧定宽</div>
	<div class="content">中间自适应</div>
    <div class="right">右侧定宽</div>
```

- relative + float + margin

``` css
<style>
	.g-sd51, .g-sd52 {
		position: relative;
		float: left;
		width: 230px;
		margin: 0 -230px 0 0;
	}
	.g-sd52 {
		float: right;
		width: 190px;
		margin: 0 0 0 -190px;
	}
	.g-mn5 {
		float: left;
		width: 100%;
	}
	.g-mn5c {
		margin: 0 200px 0 240px;
	}
</style>
```

``` html
    <div class="g-sd51">
        <p>左侧定宽</p>
    </div>
    <div class="g-mn5">
        <div class="g-mn5c">
            <p>中间自适应</p>
        </div>
    </div>
    <div class="g-sd52">
        <p>右侧定宽</p>
    </div>

```

#### 圣杯布局
- 中间栏放到文档流前面，保证先行渲染
- 三栏全部float:left浮动
- 中间栏在添加相对定位，并配合left和right属性

> 效果上表现为三栏是单独分开的

[可参考该篇圣杯的分析过程：【CSS】 布局之圣杯布局](http://www.cnblogs.com/linxiong945/p/4041841.html))

``` css
<style>
	#hd {
		height: 50px;
		background: #666;
		text-align: center;
	}
	#bd {
		/*左右栏通过添加负的margin放到正确的位置了，此段代码是为了摆正中间栏的位置*/
		padding: 0 200px 0 180px;
		height: 100px;
	}
	#middle {
		float: left;
		width: 100%;
		height: 100px;
		background: blue;
	}
	#left {
		float: left;
		width: 180px;
		height: 100px;
		margin-left: -100%;  /*左栏上去到第一行*/
		background: #0c9;
		position: relative;
		left: -180px;  /*中间栏的位置摆正之后，左栏的位置也相应右移，通过相对定位的left恢复到正确位置*/
	}
	#right {
		float: left;
		width: 200px;
		height: 100px;
		margin-left: -200px;
		background: #0c9;
		position: relative;
		right: -200px;  /*中间栏的位置摆正之后，右栏的位置也相应左移，通过相对定位的right恢复到正确位置*/
	}
	#footer {
		height: 50px;
		background: #666;
		text-align: center;
	}
</style>
```

```
<div id="hd">header</div>
<div id="bd">
  <div id="middle">middle</div>
  <div id="left">left</div>
  <div id="right">right</div>
</div>
<div id="footer">footer</div>
```


#### 双飞翼布局

- 中间栏放到文档流前面，保证先行渲染
- 三栏全部float:left浮动
- 在中间栏的div中嵌套一个div，内容写在嵌套的div里，然后对嵌套的div设置margin-left和margin-right

> 效果上表现为左右两栏在中间栏的上面
- **区别**：双飞翼多了1个div，少用大致4个css属性（圣杯布局中间div padding-left和padding-right这2个属性，加上左右两个div用相对布局position: relative及对应的right和left共4个属性，一共6个；而双飞翼布局子div里用margin-left和margin-right共2个属性，6-2=4）
作者：吕延庆
链接：https://www.zhihu.com/question/21504052/answer/50053054
来源：知乎
著作权归作者所有，转载请联系作者获得授权。

``` css
<style>
	#hd {
		height: 50px;
		background: #666;
		text-align: center;
	}
	#middle {
		float: left;
		width: 100%;
		height: 100px;
		background: blue;
	}
	#left {
		float: left;
		width: 180px;
		height: 100px;
		margin-left: -100%;   /*左栏上去到第一行*/
		background: #0c9;
	}
	#right {
		float: left;
		width: 200px;
		height: 100px;
		margin-left: -200px;
		background: #0c9;
	}

	/*给内部div添加margin，把内容放到中间栏，其实整个背景还是100%*/
	#inside {
		margin: 0 200px 0 180px;
		height: 100px;
	}
	#footer {
		clear: both; /*记得清除浮动*/
		height: 50px;
		background: #666;
		text-align: center;
	}
</style>
```

``` html
<div id="hd">header</div>
<div id="middle">
	<div id="inside">middle</div>
</div>
<div id="left">left</div>
<div id="right">right</div>
<div id="footer">footer</div>
```

### 2. 左侧自适应，中间右侧定宽
``` css
<style>
	.g-sd41, .g-sd42 {
		position: relative;
		float: right;
		width: 190px;
	}
	.g-sd41 {
		width: 230px;
		margin-left: 10px;
	}
	.g-mn4 {
		float: left;
		width: 100%;
		margin-right: -430px;
	}
	.g-mn4c {
		margin-right: 440px;
	}
</style>
```

``` html
    <div class="g-mn4">
        <div class="g-mn4c">
            <p>左侧自适应</p>
        </div>
    </div>
    <div class="g-sd41">
        <p>中间定宽</p>
    </div>
    <div class="g-sd42">
        <p>右侧定宽</p>
    </div>
```

### 3. 右侧自适应，中间左侧定宽
``` css
<style>
	.g-sd31, .g-sd32 {
		position: relative;
		float: left;
		width: 230px;
	}
	.g-sd31 {
		width: 190px;
		margin-right: 10px;
	}
	.g-mn3 {
		float: right;
		width: 100%;
		margin-left: -430px;
	}
	.g-mn3c {
		margin-left: 440px;
	}
</style>
```

``` html
    <div class="g-sd31">
        <p>左侧定宽</p>
    </div>
    <div class="g-sd32">
        <p>中间定宽</p>
    </div>
    <div class="g-mn3">
        <div class="g-mn3c">
            <p>右侧自适应</p>
        </div>
    </div>
```

## 五. 图片绝对居中

``` css
<style>
	.box {
		width: 500px;
		height: 600px;
		margin: 10px auto;
		border: 1px solid red;
		text-align: center;
	}
	.box img {
		vertical-align: middle;
	}
	.box span {
		display: inline-block;
		height: 100%;
		vertical-align: middle;
	}
</style>
```

``` html
<div class="box">
	<img src="img/xxx.png" alt=""/>
    <span></span>
</div>
```

###  图片居中溢出隐藏

``` css
<style>
	.m-demo {
		position: relative;
		width: 300px;
		height: 300px;
		overflow: hidden;
		border: 1px solid #ddd;
	}
	.m-demo p {
		position: absolute;
		top: 50%;
		left: 50%;
		margin: 0;
		padding: 0;
	}
	.m-demo img {
		position: absolute;
		top: -50%;
		left: -50%;
		display: block;
	}
	.m-demo img.hidden {
		visibility: hidden;
		position: static;
	}
</style>
```

``` html
<div class="m-demo">
    <p>
        <img src="http://nec.netease.com/img/s/1.jpg" class="hidden"/>
        <img src="http://nec.netease.com/img/s/1.jpg" alt=""/>
    </p>
</div>
<div class="m-demo">
    <p>
        <img src="http://nec.netease.com/img/m/1.jpg" class="hidden"/>
        <img src="http://nec.netease.com/img/m/1.jpg" alt=""/>
    </p>
</div>
<div class="m-demo">
    <p>
        <img src="http://nec.netease.com/img/l/1.jpg" class="hidden"/>
        <img src="http://nec.netease.com/img/l/1.jpg" alt=""/>
    </p>
</div>
```

## 六. 两列三列自适应

``` css
<style>
	/* 两列布局  主列左 侧列右 */
	.g-mn1 {
		float: left;
		width: 100%;
		margin-right: -200px;
	}
	.g-mnc1 {
		margin-right: 210px;
	}
	.g-sd1 {
		float: right;
		width: 200px;
	}

	/* 两列布局  主列右 侧列左*/
	.g-mn2 {
		float: right;
		width: 100%;
		margin-left: -200px;
	}
	.g-mnc2 {
		margin-left: 210px;
	}
	.g-sd2 {
		float: left;
		width: 200px;
	}

	/* 三列布局  主列右 两侧列左*/
	.g-mn3 {
		float: right;
		width: 100%;
		margin-left: -520px;
	}
	.g-mnc3 {
		margin-left: 520px;
	}
	.g-sd3a {
		float: left;
		width: 300px;
		margin-right: 10px;
	}
	.g-sd3b {
		float: left;
		width: 200px;
	}

	/* 三列布局  主列中 两侧列分居左右*/
	.g-mn4 {
		float: right;
		width: 100%;
		margin-left: -200px;
	}
	.g-mnc4 {
		margin-left: 210px;
	}
	.g-sd4 {
		float: left;
		width: 200px;
	}
	.g-mn5 {
		float: left;
		width: 100%;
		margin-right: -200px;
	}
	.g-mnc5 {
		margin-right: 210px;
	}
	.g-sd5 {
		float: right;
		width: 200px;
	}
</style>
```

``` html
<div class="g-bd">
    <div class="g-mn1">
        <div class="g-mnc1">
            <p>主列1内容区</p>
        </div>
    </div>
    <div class="g-sd1">
        <p>侧列1内容区</p>
    </div>
</div>
<div class="g-bd">
    <div class="g-mn2">
        <div class="g-mnc2">
            <p>主列2内容区</p>
        </div>
    </div>
    <div class="g-sd2">
        <p>侧列2内容区</p>
    </div>
</div>
<div class="g-bd">
    <div class="g-mn3">
        <div class="g-mnc3">
            <p>主列3内容区</p>
        </div>
    </div>
    <div class="g-sd3a">
        <p>侧列a内容区</p>
    </div>
    <div class="g-sd3b">
        <p>侧列b内容区</p>
    </div>
</div>
<div class="g-bd">
    <div class="g-mn4">
        <div class="g-mnc4">
            <div class="g-mn5">
                <div class="g-mnc5">
                    <p>主列5内容区</p>
                </div>
            </div>
            <div class="g-sd5">
                <p>侧列5内容区</p>
            </div>
        </div>
    </div>
    <div class="g-sd4">
        <p>侧列4内容区</p>
    </div>
</div>
```

## 七. 全屏自适应

- absolute

``` css
<style>
	html, body {
		width: 100%;
		height: 100%;
		overflow: hidden;
		margin: 0;
	}
	html {
		_height: auto;
		_padding: 100px 0 50px;
	}
	.g-hd, .g-sd, .g-mn, .g-ft {
		position: absolute;
		left: 0;
	}
	.g-hd, .g-ft {
		width: 100%;
	}
	.g-sd, .g-mn {
		top: 100px;
		bottom: 50px;
		_height: 100%;
		overflow: auto;
	}
	.g-hd {
		top: 0;
		height: 100px;
	}
	.g-sd {
		width: 300px;
	}
	.g-mn {
		_position: relative;
		left: 300px;
		right: 0;
		_top: 0;
		_left: 0;
		_margin-left: 300px;
	}
	.g-ft {
		bottom: 0;
		height: 50px;
	}
</style>
```

``` html
<div class="g-hd">
    顶部
</div>
<div class="g-sd">
    内容左侧
</div>
<div class="g-mn">
    内容右侧
</div>
<div class="g-ft">
    底部
</div>
```

- flex

``` css
<style>
	html, body, .wrapper {
		height: 100%;
		overflow: hidden;
	}
	.wrapper {
		display: flex;
		flex-direction: column;
	}
	.g-hd {
		height: 100px;
	}
	.g-ft {
		height: 50px;
	}
	.g-md {
		flex: 1;
		display: flex;
	}
	.g-md .g-sd {
		width: 200px;
	}
	.g-md .g-mn {
		flex: 1;
		overflow: auto;
	}
</style>
```

``` html
<div class="wrapper">
	<div class="g-hd">
	    顶部
	</div>
	<div class="g-md">
		<div class="g-sd">
		    内容左侧
		</div>
		<div class="g-mn">
		    内容右侧
		</div>
	</div>
	<div class="g-ft">
	    底部
	</div>
</div>
```

## 八. 底部显示自适应

``` css
<style>
	html, body {
		width: 100%;
		height: 100%;
		margin: 0;
	}
	.g-doc {
		position: relative;
		min-height: 100%;
		_height: 100%;
	}
	.g-bd {
		padding: 0 0 60px;
		zoom: 1;
	}
	.g-ft {
		height: 50px;
		margin: -50px 0 0;
		background: #ddd;
	}
</style>
```

``` html
<div class="g-doc">
    <div class="g-bd">
        <p>请增加或减少内容，或改变窗口大小，以便查看效果。</p>
        <p>请增加或减少内容，或改变窗口大小，以便查看效果。</p>
        <p>请增加或减少内容，或改变窗口大小，以便查看效果。</p>
        <p>请增加或减少内容，或改变窗口大小，以便查看效果。</p>
        <p>请增加或减少内容，或改变窗口大小，以便查看效果。</p>
    </div>
</div>
<div class="g-ft">
    <p>底部始终在文档末尾，当文档较长时跟随在文档末尾，当文档较短时在窗口底部。</p>
</div>

```

## 九. 前自适应后跟随

``` css
<style>
	.m-demo {
		padding: 5px 0;
		border-bottom: 1px dotted #ddd;
		font-size: 12px;
	}
	.m-demo .cnt {
		float: left;
		margin-right: 80px;
	}
	.m-demo .date {
		display: inline;
		float: left;
		margin-left: -70px;
	}
</style>
```

``` html
<div class="m-demo f-cb">
    <div class="cnt">这是一段长度未知的文本，自动换行，且不会把右边的时间挤掉，修改这段文字长度或改变窗口宽度试试。</div>
    <div class="date">2009-08-08</div>
</div>
```

## 十. 图文问题

``` css
<style>
	/* 左图右文  */
	.m-demo:after, .m-demo li .cnt:after {
		display: block;
		clear: both;
		visibility: hidden;
		height: 0;
		overflow: hidden;
		content: ".";
	}
	.m-demo, .m-demo li .cnt {
		zoom: 1;
	}
	.m-demo {
		width: 640px;
		margin: 0 0 1em;
		overflow: hidden;
		background: #dfedf0;
	}
	.m-demo ul {
		padding: 0;
		margin: -11px 0 -10px;
	}
	.m-demo li {
		padding: 10px 0;
		border-top: 1px dashed #999;
	}
	.m-demo .img {
		float: left;
		width: 100px;
		height: 100px;
		padding: 5px;
		border: 1px solid #ccc;
		margin-right: -112px;
		background: #eee;
	}
	.m-demo .img img, .m-demo .img a {
		display: block;
		width: 100px;
		height: 100px;
	}
	.m-demo .txt {
		line-height: 18px;
		color: #666;
		margin-left: 122px;
	}
	.m-demo .txt h3 {
		margin: 0 0 3px;
		font-size: 14px;
	}
	.m-demo .txt a, .m-demo .txt a:hover {
		color: #f60;
	}
	.m-demo .txt p {
		font-size: 12px;
		margin: 0;
	}

	/* 左图右文列表 */
	.m-demo-1 ul {
		margin: -21px 0 0 -20px;
	}
	.m-demo-1 li {
		float: left;
		display: inline;
		width: 200px;
		overflow: hidden;
		margin: 1px 0 -11px;
		padding: 20px 0 10px 20px;
		border-top: none;
		border-bottom: 1px dashed #999;
	}

	/* 上图下文列表 */
	.m-demo-2 ul {
		margin: -20px 0 0 -20px;
	}
	.m-demo-2 li {
		float: left;
		display: inline;
		width: 112px;
		padding: 0;
		border: none;
		margin: 20px 0 0 20px;
		overflow: hidden;
	}
	.m-demo-2 .img {
		float: none;
	}
	.m-demo-2 .txt {
		margin: 6px 0 0 0;
	}
</style>
```

``` html
<div class="m-demo">
    <ul>
       <li>
            <div class="cnt">
                <div class="img"><a href="#"><img src="http://nec.netease.com/img/s/3.jpg"></a></div>
                <div class="txt">
                    <h3><a href="#">标题标题标题标题</a></h3>
                    <p>左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文</p>
                </div>
            </div>
        </li>
        <li>
            <div class="cnt">
                <div class="img"><a href="#"><img src="http://nec.netease.com/img/s/3.jpg"></a></div>
                <div class="txt">
                    <h3><a href="#">标题标题标题标题</a></h3>
                    <p>左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文左图右文</p>
                </div>
            </div>
        </li> 
    </ul>
</div>
<div class="m-demo m-demo-1">
    <ul>
        <li>
            <div class="cnt">
                <div class="img"><a href="#"><img src="http://nec.netease.com/img/s/3.jpg"></a></div>
                <div class="txt">
                    <h3><a href="#">标题标题标题标题</a></h3>
                    <p>左图右文列表左图右文列表左图右文列表左图右文列表左图右文列表</p>
                </div>
            </div>
        </li>
        <li>
            <div class="cnt">
                <div class="img"><a href="#"><img src="http://nec.netease.com/img/s/3.jpg"></a></div>
                <div class="txt">
                    <h3><a href="#">标题标题标题标题</a></h3>
                    <p>左图右文列表左图右文列表左图右文列表左图右文列表左图右文列表</p>
                </div>
            </div>
        </li>
        <li>
            <div class="cnt">
                <div class="img"><a href="#"><img src="http://nec.netease.com/img/s/3.jpg"></a></div>
                <div class="txt">
                    <h3><a href="#">标题标题标题标题</a></h3>
                    <p>左图右文列表左图右文列表左图右文列表左图右文列表左图右文列表</p>
                </div>
            </div>
        </li>
    </ul>
</div>
<div class="m-demo m-demo-2">
    <ul>
        <li>
            <div class="cnt">
                <div class="img"><a href="#"><img src="http://nec.netease.com/img/s/3.jpg"></a></div>
                <div class="txt">
                    <h3><a href="#">标题标题标题</a></h3>
                    <p>上图下文列表上图下文列表上图下文列表上图下文列表</p>
                </div>
            </div>
        </li>
        <li>
            <div class="cnt">
                <div class="img"><a href="#"><img src="http://nec.netease.com/img/s/3.jpg"></a></div>
                <div class="txt">
                    <h3><a href="#">标题标题标题</a></h3>
                    <p>上图下文列表上图下文列表上图下文列表</p>
                </div>
            </div>
        </li>
        <li>
            <div class="cnt">
                <div class="img"><a href="#"><img src="http://nec.netease.com/img/s/3.jpg"></a></div>
                <div class="txt">
                    <h3><a href="#">标题标题标题</a></h3>
                    <p>上图下文列表上图下文列表上图下文列表上图下文列表</p>
                </div>
            </div>
        </li>
        <li>
            <div class="cnt">
                <div class="img"><a href="#"><img src="http://nec.netease.com/img/s/3.jpg"></a></div>
                <div class="txt">
                    <h3><a href="#">标题标题标题</a></h3>
                    <p>上图下文列表上图下文列表上图下文列表上图下文列表上图下文列表上图下文列表上图下文列表</p>
                </div>
            </div>
        </li>
        <li>
            <div class="cnt">
                <div class="img"><a href="#"><img src="http://nec.netease.com/img/s/3.jpg"></a></div>
                <div class="txt">
                    <h3><a href="#">标题标题标题</a></h3>
                    <p>上图下文列表上图下文列表上图下文列表上图下文列表上图下文列表</p>
                </div>
            </div>
        </li>
    </ul>
</div>


```

## 十一. 表头固定内容滚动

``` css
<style>
	.m-demo {
		margin: 0 0 20px;
		line-height: 18px;
	}
	.m-demo .scroll {
		max-height: 116px;
		border: 1px solid #ddd;
		border-top: 0;
		overflow-y: auto;
	}
	.m-demo table {
		width: 100%;
		table-layout: fixed;
	}
	.m-demo th, .m-demo td {
		width: 100px;
		padding: 10px;
		border: 1px solid #ddd;
	}
	.m-demo th {
		font-weight: bold;
		background: #eee;
	}
	.m-demo thead th:last-child, .m-demo tbody td:last-child {
		width: auto;
	}
	.m-demo tbody tr:nth-child(2n) {
		background: #fafafa;
	}
	.m-demo tbody tr:first-child td {
		border-top: 0;
	}
	.m-demo tbody tr:last-child td {
		border-bottom: 0;
	}
	.m-demo tbody tr td:first-child {
		border-left: 0;
	}
	.m-demo tbody tr td:last-child {
		border-right: 0;
	}
</style>
```

``` html
<div class="m-demo">
	<table>
        <thead>
           <tr><th>定宽a</th><th>定宽b</th><th>定宽c</th><th>最后列不定宽d</th></tr>
        </thead>
    </table>
	<div class="scroll">
	    <table>
            <tbody>
                <tr><td>定宽a</td><td>定宽b</td><td>定宽c</td><td>最后列不定宽d</td></tr>
                <tr><td>定宽a</td><td>定宽b</td><td>定宽c</td><td>最后列不定宽d</td></tr>
                <tr><td>定宽a</td><td>定宽b</td><td>定宽c</td><td>最后列不定宽d</td></tr>
                <tr><td>定宽a</td><td>定宽b</td><td>定宽c</td><td>最后列不定宽d</td></tr>
                <tr><td>定宽a</td><td>定宽b</td><td>定宽c</td><td>最后列不定宽d</td></tr>
                <tr><td>定宽a</td><td>定宽b</td><td>定宽c</td><td>最后列不定宽d</td></tr>
            </tbody>
        </table>
    </div>
</div>

```

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)