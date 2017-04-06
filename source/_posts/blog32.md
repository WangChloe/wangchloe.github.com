---
title: 每天10个前端知识点：HTML5(video、FileReader)
date: 2017-04-02 03:27:35
categories: 前端札记
tags: [HTML5]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

- H5视频 video
    - \(1\) 属性
    - \(2\) 方法
- H5文件 FileReader
    - \(1\) 文件拖拽
    - \(2\) File接口
    - \(3\) 示例

<!-- more -->

---


## 9. H5视频 video
不同浏览器支持的格式不同，可能需要转码，在source标签里引入多种格式

FF不支持mp4，支持ogg

### (1) 属性
- src -> 视频路径
- controls -> 显示自带控制进度条
- loop -> 视频循环
- autoplay -> 自动播放
- muted -> 静音

- currentTime -> 当前播放时间
- duration -> 视频总时间
- volume -> 音量 [0,1]

- ontimeupdate -> 进度更新

- play -> 是否在播放 返回true/false
- pause -> 是否暂停 返回true/false

### (2) 方法
- play() -> 播放视频
- pause() -> 暂停视频
- load() -> 重新加载视频
- onended() -> 视频播放完毕

常见结构
``` html
<html>
    <video src="xxx.mp4" controls></video>
    <hr>
    <video  controls>
        <source src="xxx.mp4">
        <source src="xxx.ogg">
    </video>
    <hr>
    <video src="xxx.mp4" id="v1" width="400px" height="200px"></video>
    <button onclick="play()">播放/暂停</button>
</html>
```

``` javascript
<script>
    var v1 = document.getElementById('v1');
    function play() {
        if(v1.paused) {
            v1.play();
        } else {
            v1.pause();
        }
    }
</script>
```

## 10. H5文件 FileReader

### (1) 文件拖拽
- ondragover -> 只要悬浮，一直触发
- ondragenter -> 进入时触发，有子节点时有问题
- ondragleave -> 离开时触发，有子节点时有问题
- ondrop -> 释放鼠标时触发，对应DOM节点的dragover事件必须取消默认事件

### (2) File接口
`var reader = new FileReader();`  新建文件读取对象

方法
- `.readAsText(file)` -> 读取文本文件
- `.readAsDataURL(file)` -> 读取多媒体

- `.onload` -> 资源读取完毕  reader.result
- `.onprogress` -> 读取进度更新时触发

`<progress max="100"></progress>`

```
<script>
    reader.onprogress = function(ev){
        oProgress.value = ev.loaded/ev.total*100;
    }
</script>
```

- `.onloadstart` -> 加载开始时触发
- `.onloadend` -> 加载结束时触发
- `.onerror` -> 出现错误时触发
- `.onabort` -> 加载过程中中止时触发

- `.abort` -> 手动中止加载


### (3) 示例

- 文本

``` javascript
<script>
    oBlock.ondrop = function(ev) {
        var file = ev.dataTransfer.files[0];

        var reader = new FileReader();

        reader.readAsText(file);    // 读取文本文件

        reader.onload = function(ev) {
            console.log(reader.result);
        }

        ev.preventDefault();
    }
</script>
```

- 多媒体

``` javascript
<script>
    oBlock.ondrop = function(ev) {
        var file = ev.dataTransfer.files[0];

        var reader = new FileReader();

        reader.readAsDataURL(file);     // 读取多媒体

        reader.onload = function(ev) {
            new Audio(reader.result).play();
        }

        ev.preventDefault();
    }
</script>
```

- 处理文本/多媒体

``` javascript
<script>
    oBlock.ondrop = function(ev) {
        var file = ev.dataTransfer.files[0];

        var reader = new FileReader();

        if (/text/.test(file.type)) {  // 处理文本

            reader.readAsText(file);

            reader.onload = function() {
                document.write(reader.result);
            }

            console.log('text');

        } else {  // 处理多媒体
            reader.readAsDataURL(file);

            reader.onload = function() {
                if (/image/.test(file.type)) {
                    console.log('image');

                    var oImage = new Image();
                    oImage.src = reader.result;

                    document.body.appendChild(oImage)

                } else if (/video/.test(file.type)) {

                    console.log('video');

                    oVideo.src = reader.result;
                    oVideo.play();

                } else {

                    console.log('audio');
                    new Audio(reader.result).play();

                }
            }
        }

        ev.preventDefault();
    }
</script>
```



---

更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！

公众号是坚持日更的，不发文的时候推送一些我觉得好用的前端网站或者看到的一些问题的解决方案，也更便于大家交流，就酱。

![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)