---
title: target新思
categories: 前端札记
tags: ['前端周边', 'js']
date: 2018-03-31 15:53:16
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---


<!-- more -->

[target](http://www.w3school.com.cn/tags/att_a_target.asp)

`target = "newwebview"`在pc浏览器的行为为第一次跳转时新开页面，第二次跳转时在上次新开页面上加载
![](https://ws3.sinaimg.cn/large/006tNc79gy1flk0rfpxkuj31960eqtc3.jpg)

若为`target="newwebview"`和`target="newwebview2"`时在pc浏览器的行为为分别新开两个页面，因为target值的自定义内容不一样

`target="_blank"`的行为一直为新开页面
![](https://ws1.sinaimg.cn/large/006tNc79gy1flk0rc424bj31ac0t2q9v.jpg)


fanliAPP对同为`target="newwebview"`的跳转做了拦截处理，行为都为新开页面

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)