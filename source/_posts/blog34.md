---
title: 每天10个前端知识点：HTML5(canvas)
date: 2017-04-06 03:27:35
categories: 前端札记
tags: [HTML5]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

## 13. H5画布 canvas

基于 JavaScript 的绘图 API

`<canvas></canvas>`

获取绘图上下文
``` javascript
<script>
    var oC = document.querySelector('canvas');
    var ctx = oC.getContext('2d');
</script>
```

<!-- more -->

ctx.
  - moveTo(x, y)  移动
  - lineTo(x, y)  划线
  - fillStyle = "color"  更改填充颜色
  - fill()  填充
  - strokeStyle = "color"  更改描边样式
  - stroke()  描边

- 矩形
ctx.
  - fillRect(x, y, w, h)  填充矩形(没有路径)
  - strokeRect(x, y, w, h)  描边矩形(没有路径)
  - rect(x, y, w, h)  举行路径
  - clearRect(x, y, w, h)  清空区域

- 弧
ctx.
  - arc(cx, cy, r, startDegree, endDegree, anticlockwise)  圆心x，圆心y，半径，起始角度(弧度制)，结束角度(弧度制)，是否逆时针
  - fill()  填充

- 文本
ctx.
  - font = "字号 字体"
  - textAlign = "left/center/right"  竖轴对齐方式
  - textBaseline ="top/middle/bottom" 横轴对齐方式
  - fillText('文字', x, y)  填充文字
  - strokeText('文字', x, y)  描边文字
  - measureText('文字')  返回文字长度(width)

ctx.
  - beginPath()  开始路径
  - closePath()  闭合路径

- 线条样式
ctx.
  - lineCap = "round/butt(默认)/square"  更改线帽
  - lineJoin = "round/miter(默认)/bevel(切角)"  更改连接点样式
  - lineWidth = 20  线宽

- 阴影
ctx.
  - shadowColor 设置或返回阴影颜色
  - shadowBlur  设置或返回阴影模糊级别
  - shadowOffsetX   设置或返回阴影距形状的水平距离
  - shadowOffsetY   设置或返回阴影距形状的垂直距离

- 渐变
ctx.
  - createLinearGradient(x0, y0, x1, y1)  创建线性渐变 起始位置 结束位置（用在画布内容上）
  - createPattern(img, "repeat|repeat-x|repeat-y|no-repeat")    在指定的方向上重复指定的元素
  - createRadialGradient(x0, y0, r0, x1, y1, r1)    创建径向渐变 起始位置 结束位置（用在画布内容上）
  - addColorStop(位置, 颜色)    规定渐变对象中的颜色和停止位置(0~1)

- 画图
ctx.
  - drawImage(img, x, y)  在画布上绘制图像、画布或视频
  - drawImage(img, x, y, width, height)
  - drawImage(img, sx, sy, swidth, sheight, x, y, width, height)  剪切图像，并在画布上定位被剪切的部分 s-裁剪

    ```
    <script>
        var oC = document.querySelector('canvas');
        var ctx = oC.getContext('2d');

        var oImage = new Image();
        oImage.src = 'xxx.png';

        oImage.onload = function() {
            ctx.drawImage(oImage, 0, 0);
        }
    </script>
    ```

- 像素操作
ctx.
  - getImageData(x, y, w, h)  画布指定矩形复制像素数据  **需服务器环境**
    imgData包括{data, width, height}  其中data是像素数组
    - red=imgData.data[0];
    - green=imgData.data[1];
    - blue=imgData.data[2];
    - alpha=imgData.data[3];
  - putImageData(imgData, x, y) 将图像数据放回画布

- 变换
ctx.
  - scale(sW, sH)   缩放当前绘图至更大或更小
  - rotate(角度*Math.PI/180)  旋转当前绘图
  - translate(x, y) 重新映射画布上的 (0,0) 位置
  > 注意translate和moveTo的区别

  - transform(a,b,c,d,e,f)  替换绘图的当前转换矩阵(相对变化)  水平缩放 水平倾斜 垂直倾斜 垂直缩放 水平移动 垂直移动
  - setTransform(a,b,c,d,e,f)   将当前转换重置为单位矩阵(不相对变化)  水平缩放 水平倾斜 垂直倾斜 垂直缩放 水平移动 垂直移动

ctx.
  - isPointInPath(x, y)  点是否在路径内

ctx.
  - save()  保存当前环境的状态
  - restore()   返回之前保存过的路径状态和属性


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！

公众号是坚持日更的，不发文的时候推送一些我觉得好用的前端网站或者看到的一些问题的解决方案，也更便于大家交流，就酱。

![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)