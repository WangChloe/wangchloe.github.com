---
title: 每天10个前端知识点：HTML5(新增元素及属性)
date: 2017-03-18 03:27:35
categories: 前端札记
tags: [HTML5]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。



---

- HTML5语法概要
- H5新增元素
	- \(1\) 结构元素
	- \(2\) 其他元素
	- \(3\) input元素类型
	- \(4\) *废除的元素
- H5新增属性
	- \(1\) 表单相关属性
	- \(2\) 链接相关属性
	- \(3\) 其他属性
	- \(4\) *废除属性
	- \(5\) 全局属性

<!-- more -->

---

[翻译-你必须知道的28个HTML5特征、窍门和技术](http://www.zhangxinxu.com/wordpress/2010/08/%E7%BF%BB%E8%AF%91-%E4%BD%A0%E5%BF%85%E9%A1%BB%E7%9F%A5%E9%81%93%E7%9A%8428%E4%B8%AAhtml5%E7%89%B9%E5%BE%81%E3%80%81%E7%AA%8D%E9%97%A8%E5%92%8C%E6%8A%80%E6%9C%AF/)

[html5shiv项目让IE6-IE9浏览器都支持HTML5中的元素](http://www.zhangxinxu.com/wordpress/2013/02/github-html5shiv-readme-translate/)

## HTML5语法概要
1. 内容类型
2. DOCTYPE声明
3. 指定字符编码
4. 可以省略标记的元素
5. 具有boolean值的元素
6. 省略行内属性赋值的引号

## 1. H5新增元素

![H5标签集合](http://ojvx9eehr.bkt.clouddn.com/H5%E6%A0%87%E7%AD%BE%E9%9B%86%E5%90%88.jpg)

![H5页面常用结构](http://ojvx9eehr.bkt.clouddn.com/H5%E9%A1%B5%E9%9D%A2%E5%B8%B8%E7%94%A8%E7%BB%93%E6%9E%84.gif)

### (1) 结构元素
#### section
对网站内容分块、分段
当容器需要被直接定义样式或通过脚本定义行为时，推荐使用div而非section

结构(标题+内容)
``` html
<section>
	<h1></h1>
	<p></p>
</section>
```
#### article
代表文档内容 独立性 可嵌套使用
可看作特殊的section元素

常见结构
``` html
<article>
	<header>
		<h1></h1>
	</header>
	<article>
		<header>
			<h2></h2>
		</header>
		<p></p>
		<footer></footer>
	</article>
	<footer></footer>
</article>
```

#### aside
用来表示当前页面或文章的附属信息(相关引用、侧边栏、广告、导航条)

常见结构
``` html
<header>
	<h1></h1>
</header>
<article>
	<h1></h1>
	<p></p>
	<aside>
		<h1></h1>
		<p></p>
	</aside>
</article>
<aside>
	<nav>
		<h2>评论</h2>
		<ul>
			<li><a href="javascipt:;"></a></li>
			<li><a href="javascipt:;"></a></li>
		</ul>
	</nav>
</aside>
```

#### nav
常见应用：导航条、侧边栏导航、页内导航、翻页操作
**不能用menu元素代替nav元素**

常见结构
``` html
<nav>
	<ul>
		<li><a href="javascript:;"></a></li>
		<li><a href="javascript:;"></a></li>
	</ul>
</nav>

<article>
	<header>
		<nav>
			<ul>
				<li><a href="javascript"></a></li>
				<li><a href="javascript"></a></li>
			</ul>
		</nav>
	</header>
	<section>
		<h1></h1>
		<p></p>
	</section>
	<section>
		<h1></h1>
		<p></p>
	</section>
	<footer>
		<a href="javascript:;"></a>
		<a href="javascript:;"></a>
	</footer>
</article>
```

#### figure
#### time
用来区分各时区或编码

常见结构
``` html
	<time datetime="2017-1-1">2017-1-1</time>
	<time datetime="2017-1-1T20:00">2017-1-1</time>
```

#### pubdate
指明发布时间

常见结构
``` html
	<article>
		<header>
			<h1></h1>
			<p>
				<time datetime="2017-1-1" pubdate>2017-1-1</time>
			</p>
		</header>
	</article>
```
#### header
header可出现多次

常见结构
``` html
<header>
	<h1></h1>
	<nav>
		<ul>
			<li><a href=""></a></li>
			<li><a href=""></a></li>
			<li><a href=""></a></li>
		</ul>
	</nav>
</header>
<article>
	<header>
		<h1></h1>
	</header>
</article>
```

#### footer
footer可出现多次
常见应用：脚注(相关链接、版权信息)

常见结构
``` html
<footer>
	<ul>
		<li><a href="">版权信息</a></li>
		<li><a href="">联系我们</a></li>
		<li><a href="">加入我们</a></li>
	</ul>
</footer>
```
#### hgroup
将标题及其子标题进行分组

常见结构
``` html
<article>
	<header>
		<hgroup>
			<h1>主标题</h1>
			<h2>子标题</h2>
		</hgroup>
		<p>
			<time datetime="2017-1-1">2017-1-1</time>
		</p>
	</header>
	<div></div>
	<footer>
	</footer>
</article>
```

#### address
在文档中呈现联系信息(作者名字、网站连接、电子邮箱、地址、手机号)

常见结构
``` html
<address>
	<a href=""></a>
	<a href=""></a>
</address>
<footer>
	<div>
		<address>
			<a href=""></a>
		</address>
		<time datetime="2017-1-1">2017-1-1</time>
	</div>
</footer>
```

#### 整体应用
``` html
<header>
	<h1>xxx</h1>
	<nav>
		<ul>
			<li><a href="#">a1</a></li>
			<li><a href="#">a2</a></li>
		</ul>
	</nav>
</header>
<article>
	<hgroup>
		<h1>主标题</h1>
		<h2>子标题</h2>
	</hgroup>
	<p>正文</p>
	<section>
		<div>
			<article>
				<h1>评论标题</h1>
				<p>评论正文</p>
			</article>
		</div>
	</section>
</article>
<footer>
	<small>版权***</small>
</footer>
```

### (2) 其他元素
#### video
不同浏览器支持的格式不同，可能需要转码，在source标签里引入多种格式

FF不支持mp4，支持ogg

controls属性显示自带控制进度条

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

#### audio
controls属性显示自带控制进度条

常见结构
``` html
<html>
	<audio src="xxx.mp3" controls></audio>  <!-- 显示音频自带播放器样式 -->
	<hr>
	<audio src="xxx.mp3" id="a1"></audio>  <!-- 只显示下面的按钮 -->
	<button onclick="play()">播放/暂停</button>
</html>
```

``` javascript
<script>
	var a1 = document.getElementById('a1');
	function play() {
		if(a1.paused) {
			a1.play();
		} else {
			a1.pause();
		}
	}
</script>
```

#### canvas

#### ...

### (3) input元素类型
- email
- url
- number
- range

- date
- time

- Date Pickers

### (4) *废除的元素
- 能用CSS替代的元素：basefont、big、center、font、s、tt、u等
- frame框架
- 只有部分浏览器支持的元素

## 2. H5新增属性

### (1) 表单相关属性
####form
form的元素可写在form外，只要指向相同id

常见结构
``` html
<form id="form1">
	<input type="text">
</form>
<textarea form="form1"></textarea>
```
#### formaction
不同表单元素不同action路径

常见结构
``` html
<form id="form1">
	<input type="text">
	<input type="submit" name="s1" value="v1" formaction="fc1">
	<input type="submit" name="s2" value="v2" formaction="fc2">
	<input type="submit" name="s3" value="v3" formaction="fc3">
</form>
```

#### formmethod
不同表单元素不同提交方法

常见结构
``` html
<form id="form1">
	<input type="text">
	<input type="submit" name="s1" value="v1" formmethod="get" formaction="fc1">
	<input type="submit" name="s2" value="v2" formmethod="post" formaction="fc2">
</form>
```
#### formenctype
不同表单元素不同编码方式

常见结构
``` html
<form id="form1">
	<input type="text" formenctype="text/plain" value="表单数据中的空格转换为加号">
	<input type="text" formenctype="multipart/form-data" value="文件上传">
	<input type="text" formenctype="application/x-www-form-urlencoded" value="get方式时把表单数据转换为字符">
</form>
<textarea form="form1"></textarea>
```

#### formtarget
不同表单元素不同提交后在何处打开加载页面

常见结构
``` html
<form id="form1">
	<input type="text">
	<input type="submit" name="s1" value="v1" formtarget="_blank" formaction="fc1">
	<input type="submit" name="s1" value="v1" formtarget="_self" formaction="fc1">
	<input type="submit" name="s1" value="v1" formtarget="_parent" formaction="fc1">
	<input type="submit" name="s1" value="v1" formtarget="_top" formaction="fc1">
	<input type="submit" name="s1" value="v1" formtarget="framename" formaction="fc1">
</form>
```

#### autofocus
表单元素自动获得焦点

常见结构
``` html
<form id="form1">
	<input type="text" autofocus>
	<input type="text">
</form>
```

#### require
提交时内容为空不允许提交，并显示提示

常见结构
``` html
<form id="form1">
	<input type="text" required>
	<input type="text">
	<input type="submit">
</form>
```
#### labels
验证提示信息

常见结构
``` html
<form id="form1">
	<label for="t1" id="l1"></label>
	<input type="text" id="t1">
	<input type="button" id="btn1" value="验证" onclick="validate()">
</form>
```

``` javascript
<script>
	function validate() {
		var t1 = document.getElementById('t1');
		var btn1 = document.getElementById('btn1');
		var fm1 = document.getElementById('form1');

		if(t1.value.trim() == "") {
			var l1 = document.getElementById('l1');
			l1.setAttribute('for', 't1');
			f1.insertBefore(l1, btn1);	// 在button前显示验证提示信息
			t1.labels[1].innerHTML = '输入为空';
		}
	}
</script>
```

#### placeholder
输入提示信息

常见结构
``` html
<form id="form1">
	<input type="text" placeholder="请输入...">
</form>
```

#### datalist
可输入的下拉框

常见结构
``` html
<form id="form1">
	<input type="text" name="n1" list="ns">  <!-- 点击下拉箭头时显示datalist -->
	<datalist id="ns" style="display: none">
		<option value="v1">v1</option>
		<option value="v2">v2</option>
	</datalist>
</form>
```
#### autocomplete
输入自动填充

#### pattern
表单元素正则验证，输入错误时不跳转

常见结构
``` html
<form id="form1" action="xxx">
	<input type="text" pattern="{a-z}[3]" >
	<input type="submit">
</form>
```

#### selectionDirection
*Chrome不支持

#### indeterminate
复选框checkbox 的第三种状态 “尚未明确是否选取”状态

#### image按钮的width/height

### (2) 链接相关属性
### (3) 其他属性
### (4) *废除属性

### (5) 全局属性
contentEditable
designMode
hidden
spellcheck       拼写检查
tabindex         设置tab键焦点的顺序


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)