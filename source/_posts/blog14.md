---
title: 每天10个前端知识点：命名规范
date: 2017-02-05 03:27:35
categories: 前端札记
tags: [前端周边]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

{% aplayer "风的季节" "Soler" "http://ojvx9eehr.bkt.clouddn.com/Soler%20-%20%E9%A3%8E%E7%9A%84%E5%AD%A3%E8%8A%82.mp3" %}


---


## 规范
### Reset.css
[张鑫旭-HTML5 css reset](http://www.zhangxinxu.com/wordpress/2010/08/html5-css-reset/)

[目前比较全的CSS重设(reset)方法总结](http://www.cnblogs.com/hnyei/archive/2011/10/04/2198779.html)

<!-- more -->


### css命名规范

头：header　　 
内容：content/container　　 
尾：footer　　 
导航：nav　　 
侧栏：sidebar 
栏目：column　　 
包裹层：wrapper　　 
左右中：left right center　　 
登录条：loginbar　　 
标志：logo　　 
抽奖：raffle
广告：banner　　 
页面主体：main　　 
热点：hot　　 
新闻：news 
下载：download　　 
子导航：subnav　　 
菜单：menu　　 
子菜单：submenu　　 
搜索：search　　 
友情链接：friendlink　　 
页脚：footer　　 
版权：copyright　　 
滚动：scroll　　 
内容：content 
标签页：tab 
文章列表：list 
提示信息：msg 
小技巧：tips 
栏目标题：title 
加入：joinus 
指南：guide 
服务：service 
注册：regsiter 
状态：status 
投票：vote 
合作伙伴：partner 

### 注释的写法 

<!-- header -->
<header>xxx</header>

<!-- end header -->

### id命名 

#### (1)页面结构 

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

#### (2)导航 

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

#### (3)功能 
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

### class的命名 

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

### css层次
主要的 main.css
模块 module.css 
基本共用 base.css/global.css
布局，版面 layout.css 
主题 themes.css 
专栏 columns.css 
文字 font.css 
表单 forms.css 
补丁 mend.css 


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)