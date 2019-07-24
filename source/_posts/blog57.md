---
title: css规范对比
categories: 前端札记
tags: ['css', '规范']
date: 2017-10-10 17:05:46
updated: 2017-10-10 17:05:46
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

<!-- MarkdownTOC -->

- 连字符分隔
- OOCSS
- NEC
	- 区分
	- 示例
- BEM
	- 区分
	- 连接
	- 示例
	- 痛点

<!-- /MarkdownTOC -->

<!-- more -->



## 连字符分隔


``` html
<div class="card">
  <div class="card-header">
    <ul class="card-header-ul">
        <li class="card-header-li">
            <p class="card-header-p"></p>
        </li>
    </ul>
  </div>
  <div class="card-content"></div>
  <div class="card-footer"></div>
</div>
```

## OOCSS
[Object Oriented CSS  面向对象的CSS](http://oocss.org/)

> 结构和样式分离

需求：有一个容器是页面page的1/4宽，有一个蓝色的背景，1px灰色的边框，10px的左右边距，5px的上边距，10px的下边距

- 非OOCSS写法

``` html
<div class="size1of4"></div>
```

``` css
.size1of4 {
	background: blue;
	border: 1px solid #ccc;
	margin: 5px 10px 10px;
	width: 25%;
}
```

- OOCSS写法
``` html
<div class="size1of4 bgBlue solidGray mts mlm mrm mbm"></div>
```

``` css
.size1of4 {width: 25%;}
.bgBlue {background:blue}
.solidGray {border: 1px solid #ccc}
.mts {margin-top: 5px}
.mrm {margin-right: 10px}
.mbm {margin-bottom: 10px}
.mlm {margin-left: 10px}
```

## NEC

[nice easy css官网](http://nec.netease.com/standard/css-sort.html)

#### 区分
- 布局（grid）`.g-`： eg:头部，尾部，主体，侧栏等

- 模块（module）`.m-`： 较大整体，eg:导航、登录注册、搜索等

- 元件（unit）`.u-`： 不可再分个体，eg:按钮、input框等

- 功能（function）`.f-`： 使用频率较高样式，eg:清除浮动

- 皮肤（skin）`.s-`： eg:文字色，背景色，边框色等

- 状态`.z-`： eg:hover，选中等

- 特殊`.j-`： 被专用于JS获取节点，请勿使用.j-定义样式。

#### 示例
[网易云阅读](http://yuedu.163.com/)

``` css
/* 布局 */
.g-sd {
  float: left;
  width: 300px;
}

/* 模块 */
.m-logo {
  width: 200px;
  height: 50px;
}

.m-logo .u-sel {

}
.m-logo.z-dis {

}

/* 元件 */
.u-btn {
  height: 20px;
  border: 1px solid #333;
}

/* 统一清除浮动 */
.f-clearfix:after {
  display: block;
  visibility: hidden;
  clear: both;
  height: 0;
  overflow: hidden;
  content: '.';
}

/* 功能 */
.f-tac {
  text-align: center;
}

/* 皮肤 */
.s-fc {
  color: #fff;
}
```

## BEM
[BEM官网](http://getbem.com/naming/)

#### 区分
- `B` 块（block）
- `E` 元素（element）
- `M` 修饰符（modifier）


![](http://www.w3cplus.com/sites/default/files/styles/print_image/public/blogs/2013/Definitions-BEM-6.jpg)

#### 连接
- `__`双下划线： 间隔 块和元素
  - `.block__elem`

- `-`中划线： 间隔块、元素、修饰符中多个单词
  - `.block-name__elem-name`
  - `.block-name--modifier-modifier1`

- `--`双中划线： 间隔 块和修饰符 或 元素和修饰符
  - `.block__elem--modifier`


#### 示例

- B嵌套E

``` html
<div class="card">
  <div class="card__header">
    123
  </div>
</div>
```

- B多层嵌套E

``` html
<div class="card">
  <div class="card__header">
    <ul class="card__header__ul">
        <li class="card__header__li">
            <p class="card__header__p"></p>
        </li>
    </ul>
  </div>
</div>
```

``` scss
.card {
	&__header {
		&__ul {

		},
		&__li {

		},
		&__p {

		}
	}
}
```

- M修饰B/E

``` html
<div class="card card--red">
    <div class="card__header card__header--blue"></div>
</div>
```

``` html
<button class="button">
	Normal button
</button>
<button class="button button--state-success">
	Success button
</button>
<button class="button button--state-danger">
	Danger button
</button>
```

#### 痛点

- 命名较长
- 会存在修饰符下的元素结构不清晰

``` css
.card {
	.&__header {

	},
	&__red {
		.card_header {

		}
	}
}
```




---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)