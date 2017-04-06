---
title: 每天10个前端知识点：前端规范
date: 2017-02-05 03:27:35
categories: 前端札记
tags: [前端周边]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

{% aplayer "风的季节" "Soler" "http://ojvx9eehr.bkt.clouddn.com/Soler%20-%20%E9%A3%8E%E7%9A%84%E5%AD%A3%E8%8A%82.mp3" %}


---

## 1. Reset.css && Normailze.css

- reset.css 的作用是清除所有的默认样式
- normalize.css 是把所有的元素样式回归本该是的样子，并且消除了不同浏览器之间默认样式的区别。

### Reset
[张鑫旭-HTML5 css reset](http://www.zhangxinxu.com/wordpress/2010/08/html5-css-reset/)

[目前比较全的CSS重设(reset)方法总结](http://www.cnblogs.com/hnyei/archive/2011/10/04/2198779.html)

### Normailze
[Normalize.css：使浏览器更一致地呈现所有元素。](http://necolas.github.io/normalize.css/)

<!-- more -->

## 2. css命名规范

**CSS内部的分类及其顺序**

1. **布局（grid）（.g-）**：将页面分割为几个大块，通常有头部、主体、主栏、侧栏、尾部等！
2. **模块（module）（.m-）**：通常是一个语义化的可以重复使用的较大的整体！比如导航、登录、注册、各种列表、评论、搜索等！
3. **元件（unit）（.u-）**：通常是一个不可再分的较为小巧的个体，通常被重复用于各种模块中！比如按钮、输入框、loading、图标等！
4. **功能（function）（.f-）**：为方便一些常用样式的使用，我们将这些使用率较高的样式剥离出来，按需使用，通常这些选择器具有固定样式表现，比如清除浮动等！不可滥用！
5. **皮肤（skin）（.s-）**：如果你需要把皮肤型的样式抽离出来，通常为文字色、背景色（图）、边框色等，非换肤型网站通常只提取文字色！非换肤型网站不可滥用此类！
6. **状态（.z-）**：为状态类样式加入前缀，统一标识，方便识别，她只能组合使用或作为后代出现（.u-ipt.z-dis{}，.m-list li.z-sel{}），具体详见命名规则的扩展相关项。

> 特殊：.j-将被专用于JS获取节点，请勿使用.j-定义样式。



示例
``` css
<style>
	/* 统一调用背景图 */
	.m-logo a,.m-nav a,.m-nav em{background:url(images/sprite.png) no-repeat 9999px 9999px;}
	/* 统一清除浮动 */
	.g-bdc:after,.m-dimg ul:after,.u-tab:after{display:block;visibility:hidden;clear:both;height:0;overflow:hidden;content:'.';}
	.g-bdc,.m-dimg ul,.u-tab{zoom:1;}
	/* 布局 */
	.g-sd{float:left;width:300px;}
	/* 模块 */
	.m-logo{width:200px;height:50px;}
	/* 元件 */
	.u-btn{height:20px;border:1px solid #333;}
	/* 功能 */
	.f-tac{text-align:center;}
	/* 皮肤 */
	.s-fc,a.s-fc:hover{color:#fff;}
</style>
```


**布局（.g-）**

| 语义	| 命名	| 简写|
|  :----: | :----: | :---:  |
| 文档	| doc	| doc|
| 头部	| head	| hd|
| 主体	| body	| bd|
| 尾部	| foot	| ft|
| 主栏	| main	| mn|
| 主栏子容器	| mainc	| mnc|
| 侧栏	| side	| sd|
| 侧栏子容器	| sidec| 	sdc|
| 盒容器	| wrap/box	| wrap/box|

**模块（.m-）、元件（.u-）**

| 语义	| 命名	| 简写|
|  :----: | :----: | :---:  |
| 导航	| nav	| nav|
| 子导航	| subnav	| snav|
| 面包屑	| crumb	| crm|
| 菜单	| menu	| menu|
| 选项卡	| tab	| tab|
| 标题区	| head/title	| hd/tt|
| 内容区	| body/content	| bd/ct|
| 列表	| list	| lst|
| 表格	| table	| tb|
| 表单	| form	| fm|
| 热点	| hot	| hot|
| 排行	| top	| top|
| 登录	| login	| log|
| 标志	| logo	| logo|
| 广告	| advertise	| ad|
| 搜索	| search	| sch|
| 幻灯	| slide	| sld|
| 提示	| tips	| tips|
| 帮助	| help	| help|
| 新闻	| news	| news|
| 下载	| download	| dld|
| 注册	| regist	| reg|
| 投票	| vote	| vote|
| 版权	| copyright	| cprt|
| 结果	| result	| rst|
| 标题	| title	| tt|
| 按钮	| button	| btn|
| 输入	| input	| ipt|

**功能（.f-）**

| 语义	| 命名	| 简写|
|  :----: | :----: | :---:  |
| 浮动清除	| clearboth	| cb|
| 向左浮动	| floatleft	| fl|
| 向右浮动	| floatright	| fr|
| 内联块级	| inlineblock	| ib|
| 文本居中	| textaligncenter	| tac|
| 文本居右	| textalignright	| tar|
| 文本居左	| textalignleft	| tal|
| 垂直居中	| verticalalignmiddle	| vam|
| 溢出隐藏	| overflowhidden	| oh|
| 完全消失	| displaynone	| dn|
| 字体大小	| fontsize	| fs|
| 字体粗细	| fontweight	| fw|

**皮肤（.s-）**

| 语义	| 命名	| 简写|
|  :----: | :----: | :---:  |
| 字体颜色	| fontcolor	| fc|
| 	背景	| background	| bg|
| 背景颜色	| backgroundcolor	| bgc|
| 背景图片	| backgroundimage	| bgi|
| 背景定位	| backgroundposition	| bgp|
| 边框颜色	| bordercolor	| bdc|

**状态（.z-）**

| 语义	| 命名	| 简写|
|  :----: | :----: | :---:  |
| 选中	| selected	| sel|
| 当前	| current	| crt|
| 显示	| show	| show|
| 隐藏	| hide	| hide|
| 打开	| open	| open|
| 关闭	| close	| close|
| 出错	| error	| err|
| 不可用	| disabled	| dis|


## 3. 注释的写法 

<!-- header -->
<header>xxx</header>

<!-- end header -->

## 4. id命名 

### (1)页面结构 

容器: container 
页头：header 
内容：content/container 
页面主体：main 
页尾：footer 
导航：nav 
侧栏：sidebar 
栏目：column 
包裹层：wrapper 
左右中：left right center 

### (2)导航 

导航：nav 
主导航：mainnav 
子导航：subnav 
顶导航：topnav 
边导航：sidebar 
左导航：leftsidebar 
右导航：rightsidebar 
菜单：menu 
子菜单：submenu 
标题: title 
摘要: summary 

### (3)功能 
标志：logo 
广告：banner 
登陆：login 
登录条：loginbar 
注册：regsiter 
搜索：search 
功能区：shop 
标题：title 
加入：joinus 
状态：status 
按钮：btn 
滚动：scroll 
标签页：tab 
文章列表：list 
提示信息：msg 
当前的: current 
小技巧：tips 
图标: icon 
注释：note 
指南：guide 
服务：service 
热点：hot 
新闻：news 
下载：download 
投票：vote 
合作伙伴：partner 
友情链接：link 
版权：copyright 

## 5. class的命名 

(1) 颜色:使用颜色的名称或者16进制代码,如 
.red { color: red; } 
.f60 { color: #f60; } 
.ff8600 { color: #ff8600; } 

(2) 字体大小,直接使用"font+字体大小"作为名称,如 
.font12px { font-size: 12px; } 
.font9pt {font-size: 9pt; } 

(3) 对齐样式,使用对齐目标的英文名称,如 

.left { float:left; } 

(4) 标题栏样式,使用"类别+功能"的方式命名,如 
.barnews { } 
.barproduct { } 

> 注意事项 
1.一律小写; 
2.尽量用英文; 
3.尽量不缩写，除非一看就明白的单词.

## 6. css层次
主要的 main.css/index.css
模块 module.css 
基本共用 base.css/global.css
布局，版面 layout.css 
主题 themes.css 
专栏 columns.css 
文字 font.css 
表单 forms.css 
补丁 mend.css 

## 7. css书写次序
显示属性 -> 自身属性 -> 文本属性和其他修饰


 | 显示属性	| 自身属性	| 文本属性和其他修饰|
 |  :----: 	|   :----: 	| :---:             |
 |	display	|margin	    |font               |
 |visibility|padding	|text-align			|
 |position  |width		|text-decoration	|
 | 	float	|height		|vertical-align		|
 |	clear	|border		|white-space		|
 |list-style|overflow	|color				|
 |	top 	|min-width	|background			|

 - 显示属性： `display`, `visibility`, `position`, `top`, `float`, `clear`, `list-style`
 - 自身属性： `margin`, `padding`, `width`, `height`, `border`, `overflow`, `min-width`
 - 文本属性： `font`, `text-align`, `text-decoration`, `vertical-align`， `white-space`, `color`, `background`

## 8. 媒体查询(media)屏幕宽度

``` css

<style>
	/* 横屏 */
	@media screen and (orientation:landscape){
	    
	}
	/* 竖屏 */
	@media screen and (orientation:portrait){
	    
	}
	/* 窗口宽度<960,设计宽度=768 */
	@media screen and (max-width:959px){
	    
	}
	/* 窗口宽度<768,设计宽度=640 */
	@media screen and (max-width:767px){
	    
	}
	/* 窗口宽度<640,设计宽度=480 */
	@media screen and (max-width:639px){
	    
	}
	/* 窗口宽度<480,设计宽度=320 */
	@media screen and (max-width:479px){
	    
	}
	/* windows UI 贴靠 */
	@media screen and (-ms-view-state:snapped){
	    
	}
	/* 打印 */
	@media print{
	    
	}
</style>

```

## 9. js常见变量命名

常见类型前缀
- `o`  object 一个对象，一个元素 eg:oDiv
- `a`  array  一组元素 eg:aLi
- `s` string 字符串 eg:sUserName
- `i` integer 整数 eg:iCount
- `f`  float 浮点数 eg:fPrice
- `b` boolean 布尔 eg:bOk
- `fn` function 函数 eg:fnSucc(成功的回调函数)
- `re` RegExp 正则 eg:reMailCheck


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)