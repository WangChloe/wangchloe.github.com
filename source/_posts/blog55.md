---
title: video.js小记
categories: 前端札记
tags: ['项目总结', 'video.js']
date: 2019-06-25 15:34:01
updated: 2019-06-25 15:34:01
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

<!-- MarkdownTOC -->

- chrome video不能自动播放
- videojs暂停后不能播放
- iOS 10+ 新政策。
- Refused to create worker from blob

<!-- /MarkdownTOC -->

<!-- more -->

[video.js使用技巧](https://www.awaimai.com/2053.html)


![](http://ww4.sinaimg.cn/large/006tNc79gy1g5hnzcn1tmj30f00trq58.jpg)

### chrome video不能自动播放

- 方法一

在chrome 浏览器中输入：chrome://flags，搜索“Autoplay policy”，默认为“Default”，

修改为 “No user gesture is required” 


- 方法二

在使用video标签的过程中，设置静音播放

### videojs暂停后不能播放

需使用source配置video资源

``` html
 <video
     id="J_video_wrap"
     class="video-js video-box"
     ref="video"
     controls
     preload="auto"
     autoplay
     x-webkit-airplay="allow"
     webkit-playsinline
     playsinline
    poster="{$video.img}"
    src="{$video.link}">
 </video>
```

``` html
 <video
     id="J_video_wrap"
     class="video-js video-box"
     ref="video"
     controls
     preload="auto"
     autoplay
     x-webkit-airplay="allow"
     webkit-playsinline
     playsinline
    poster="{$video.img}">
    <source src="{$video.link}" type="video/mp4">
 </video>

```

### iOS 10+ 新政策。
在省电模式下无法自动播放视频。android任何模式都不能自动播放，只能让用户手动触发


###  Refused to create worker from blob

![](http://ww3.sinaimg.cn/large/006tNc79gy1g5b2chlepnj30tj01xdgf.jpg)

解决：

![](http://ww2.sinaimg.cn/large/006tNc79gy1g5b2ckq6f6j30tj06l3zq.jpg)

在header的CSP(Content Security Policy ,内容安全策略)处添加blob:

[前端安全配置之Content-Security-Policy(csp)](https://www.cnblogs.com/heyuqing/p/6215761.html)


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)