---
title: 每天10个前端知识点：性能优化篇
date: 2017-02-24 03:27:35
categories: 前端札记
tags: [js, css, 性能优化]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。



---

二话不说！先三炷香膜拜下**YaHoo军规**。

![YaHoo军规](https://img.aotu.io/wangcainuan/2016-03-16-optimization/%E9%9B%85%E8%99%8E35%E6%9D%A1%E5%86%9B%E8%A7%84.jpg)

<!-- more -->

以下方便你们ctrl+c、ctrl+v

- 网页内容
  - 减少 http请求次数
  - 减少 DNS查询次数
  - 避免页面跳转
  - 缓存 Ajax
  - 延迟加载
  - 提前加载
  - 减少 DOM元素数量
  - 根据域名划分内容
  - 减少iframe数量
  - 避免 404
- 服务器
  - 使用CDN(内容分发网络)
  - 添加Expires或Cache-Control报文头
  - Gzip压缩传输文件
  - 配置ETags
  - 尽早flush(刷新输出)缓冲
  - 使用GET来完成AJAX请求
  - 避免空的图片src
- Cookie
  - 减少Cookie大小
  - 页面内容使用无cookie域名
- CSS
  - 将样式表置顶
  - 避免使用CSS表达式(Expression)
  - 用<link>代替@import
  - 避免使用Filters(滤镜)
- JavaScript
  - 把脚本置于页面底部
  - 使用外部JavaScript和CSS
  - 精简JavaScript和CSS
  - 去除重复脚本
  - 减少DOM访问
  - 开发智能事件处理程序
- 图片
  - 优化图像
  - 优化CSS Spirite
  - 不要在HTML中缩放图片
  - favicon.ico要小而且可缓存
- 移动客户端
  - 保持单个内容小于25KB
  - 打包组建成复合文档


>再来一张私藏的移动端性能优化

![移动H5性能优化指南.jpeg](http://upload-images.jianshu.io/upload_images/2125655-200c9922b07ce645.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


> 以下是正文

## 1. 性能优化
项目讲求：
1. 稳定性
2. 可维护性
3. 性能优化

性能分类：
- 执行性能
- 网络性能

## 2. 重排 && 重绘

[简单-页面性能优化](http://www.jianshu.com/p/3802ac513bc5)
[你真的了解回流和重绘吗](https://github.com/chenjigeng/blog/blob/master/%E4%BD%A0%E7%9C%9F%E7%9A%84%E4%BA%86%E8%A7%A3%E5%9B%9E%E6%B5%81%E5%92%8C%E9%87%8D%E7%BB%98%E5%90%97.md)

### (1) 重排(reflow)
即重新生成布局，**重排必然导致重绘**。

会触发重排的属性：

- 盒子模型相关属性
  - width
  - height
  - padding
  - margin
  - display
  - border-width
  - border
  - min-height
- 定位属性及浮动
  - top
  - bottom
  - left
  - right
  - position
  - float
  - clear
- 改变节点内部文字结构
  - text-align
  - overflow-y
  - font-weight
  - overflow
  - font-family
  - line-height
  - vertival-align
  - white-space
  - font-size

### (2) 重绘
即重新绘制，**重绘不一定需要重排**。

会触发重绘的属性：

- color
- border-style
- border-radius
- visibility
- text-decoration
- background
- background-image
- background-position
- background-repeat
- background-size
- outline-color
- outline
- outline-style
- outline-width
- box-shadow

## 3. 图层

浏览器layout和paint是在每一个图层上进行的，当有一个元素经常变化，为了减少这个元素对页面的影响，我们可以为这个元素创建一个单独的图层，来提供页面的性能。

- 什么时候会创建图层
  - 3D或透视变换（perspective transform）CSS属性（例如translateZ(0)/translate3d(0,0,0)）
  - 使用加速视频解码的`<video>`节点
  - 拥有3D（WebGL）上下文或加速的2D上下文的`<canvas>`节点
  - 混合插件（如Flash）
  - 对自己的opacity做CSS动画或使用一个动画webkit变换的元素
  - 拥有加速CSS过滤器的元素
  - 元素有一个包含复合层的后代节点（一个元素拥有一个子元素，该子元素在自己的层里）
  - 元素有一个z-index较低且包含一个复合层的兄弟元素（换句话说就是该元素在复合层上面渲染）
  - position为fixed也会创建图层，而absolute则不会

利用GPU来加速页面渲染(硬件加速)

- 触发CSS硬件加速
  - translate3d(0,0,0)
  - rotate3d(0,0,0,0)
  - scale3d(0,0,0)
  - translateZ(0)【可能】

## 4. 执行性能优化
建议：
1. 不用的东西删除
2. 尽量不用全局变量(命名冲突，耗资源)
3. 能用系统自带的一定不用自己写的(eg:getByClass)
4. 尽量使用正则操作字符串

5. DOM操作耗性能，尽量减少DOM操作
6. 属性用的越多，性能越低
7. css耗性能属性：expression、filter、border-radius、box-shadow、gradients

> 1. Math.floor比parseInt快
2. 变量性能消耗：属性 > 全局变量 > 局部变量

## 5. 网络性能优化

### 查看网络性能 F12 -> network
- Status 状态码
- Type   资源类型
  - 主类型/次类型
  - text/html
  - text/css
  - image/png/gif
- Initiator  发起人
- Size   资源大小
- Time   请求耗时
- Timeline  时间轴
  - Queueing  排队
  - Stalled   停滞
  - Request sent  请求发送
  - Waiting   等待
  - Content Download  内容下载

#### 工具
FF -> firebug -> yslow(网站评分)
Chrome -> Audits(查看网络性能)

### 网络性能提升方法
1. 减少http请求 -> 合并
  - js -> 合并
  - css -> 合并，避免`@import`方式引入css文件
  - 图片 -> css sprites

2. http请求越小越好 -> 压缩
  - js -> 压缩
  - css -> 压缩
    - css值缩写(margin,border等)
    - 省略值为0的单位
    - 色值用最短的表示
  - 图片 -> 压缩
  工具
    - [在线工具 —— 开源中国社区](在http://tool.oschina.net/)
    - [HTML格式化 、HTML压缩- 站长工具](http://www.sojson.com/jshtml.html)
    - 前端自动化工具

3. 图片延迟加载
4. CDN(Content Delivery Network, 内容分发网络)加速
5. 加载顺序
  - 阻塞加载(同步加载)
    js
    **解决**：引用其他网站的js放在body最后
  - 非阻塞加载(异步加载，并行加载)
    css、html、图片

## 6. 垃圾回收 GC(Gabage Collection)
- js中的垃圾回收：(宿主环境)
浏览器会自动回收垃圾

- 底层语言不会自动回收垃圾

垃圾的评判标准：**生存周期**

生存周期：作用域
1. 全局变量：生存周期长，直到浏览器关闭时清除  **占资源**
2. 局部变量：生存周期短，方法调用完即清除
3. 闭包(子函数可以使用父函数的全局变量)
  子函数若没有释放，整条作用域链上的局部变量都会保留
  作用域链：从内一级一级往外找，知道全局

``` javascript
<script>
	// 作用域链
	function show1() {
		var a = 12;
		function show2() {
			var b = 1;
			function show3() {
				var c = 2;
				document.onclick = function() {
					var d = 3;
					alert(a);
				}
			}
			show3();
		}
		show2();
	}

	show1();
</script>
```

## 7. 递归

函数自己调用自己
核心思想：化大为小，逐一解决

> **斐波那契数列(Fibonacci sequence)**

> 以递归的方法定义：
F0=0，F1=1，Fn=F(n-1)+F(n-2)（n>=2，n∈N*）

**经典问题：兔子问题**

有一对兔子，从出生后第3个月起每个月都生一对兔子，小兔子长到第三个月后每个月又生一对兔子，假如兔子都不死，问每个月的兔子总对数为多少？

**分析**：
假设将兔子分为小、中、大三种。
兔子从出生后每三个月就生出一对兔子，那么我们假定第一个月为小兔子，第二个月为中兔子，第三个月之后就为老兔子(老兔子每过三个月还会再生的)。

那么第一个月分别有1、0、0，第二个月分别为0、1、0，第三个月分别为1、0、1，第四个月分别为1、1、1，第五个月分别为2、1、2，第六个月分别为3、2、3，第七个月分别为5、3、5……

兔子总对数分别为：1、1、2、3、5、8、13……

找出规律即得出以下代码


``` javascript
<script>
  function fn(n) {  // n为当前月份
        var arr = [];
    if(n <= 2) {  // 前两个月只有一对兔子
      return 1;
    } else {
      if(arr[n]) {
        return arr[n];
      } else {
        arr[n] = fn(n-1) + fn(n-2);  // 第三个月开始返回前两月之和
        return arr[n];  // 返回截止当前月份的总对数
      }
    }
  }
</script>
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)