---
title: Intellij IDEA建立mac下java开发环境
date: 2019-04-05 03:27:35
categories: 前端札记
tags: [java]

---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---


- `commond+T` 拉取分支
- `commond+k` 提交分枝
- `shift+shift` 全局搜索文件名
- `control+shift+f` 全局搜索内容(.do)
- `command+command` maven projects
<!-- more -->

- 文件名 mall.properties 本地查看html效果
- 文件名 page-config.xml 配置css、js
- 新建分支

![新建分支](https://ws1.sinaimg.cn/large/006tNc79gy1fm5o3mjyi1j30l10om114.jpg)
![新建分支](http://ww3.sinaimg.cn/large/006tNc79gy1g5awgmpl9qj30nc0qetgv.jpg)
> 新建分支后更改mall.properties文件内容，增加端口号

```
mall.common.api.url=http://service.shzyfl.cn:7101
```

- 端口占用解决方法
终端输入占用端口号10100
```
lsof -i:10100
```

显示
```
COMMAND  PID    USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
java    1372 wangjie   18u  IPv6 0x40969a7cc34a9943      0t0  TCP *:itap-ddtp (LISTEN)

```

kill该端口进程PID
```
kill -9 1372
```



---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)