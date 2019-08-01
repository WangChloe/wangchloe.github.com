---
title: 每天10个前端知识点：面向对象(下)
date: 2017-02-28 03:27:35
categories: 前端札记
tags: [js]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。



---

## 7. 如何编写面向对象程序
1. 采用构造+原型，写一个构造函数
2. 把方法挂在原型上(不能有方法嵌套)
3. 把全局变量变成属性
4. 调整this

<!-- more -->

```
<script>
	// 1. 写一个构造函数
	function ToRed() {
		// 3. 全局变量变成属性
		this.oDiv = document.body.children[0];

		// document.onclick = this.fnClick;
		// 上句相当于
		// document.onclick = function() {
		// 	alert(this);	// document
		// 	this.oDiv.style.background = '#f00';	// 当前的this指向document，错误
		// }

		// 4. 调整this
		var _this = this;
		document.onclick = function() {
			_this.fnClick();
		}
	}

	// 2. 方法挂在原型上
	ToRed.prototype.fnClick = function() {
		this.oDiv.style.background = '#f00';
	}

	new ToRed();
</script>
```

## 8. 面向对象：继承
1. 属性的继承

- `父级的构造函数.call(this, 参数1, 参数2, ...);`
- `父级的构造函数.apply(this, arguments);`

2. 方法的继承

- `子级.prototype = 父级.prototype;`  // 引用  子级新方法在此之前写会被清空
  问题：子级改了，父级也改了

- 循环复制

```
<script>
	for(var name in 父级.prototype) {
		子级.prototype[name] = 父级.prototype[name];  // 循环复制父级的方法
	}
</script>
```
问题：`子级 instanceof 父级;`  // false

- 组合继承：子级的原型对象指向父级的实例，子级的原型对象的构造函数再指向自己。 **推荐使用**

```
<script>
	子级.prototype = new 父级的构造函数(); // 此时子级.prototype.constructor与父级相等
	子级.prototype.constructor = 子级的构造函数;
</script>
```

- 寄生组合式继承(JS高程第3版 第6章)

``` javascript
function SuperType(name) {
    this.name = name
    this.colors = ['red']
}

SuperType.prototype.sayName = function() {
    console.log(this.name)
}
// 继承实例属性
function SubType(name, age) {
    SuperType.call(this, name)
    this.age = age
}

function inheritPrototype(subType, superType) {
    let prototype = Object.create(superType.prototype)
    prototype.constructor = subType
    subType.prototype = prototype
}
// 继承原型方法
inheritPrototype(SubType, SuperType)

// 定义自己的原型方法
SubType.prototype.sayAge = function() {
    console.log(this.age)
}
```


### 实例：自动播放选项卡(继承)

``` css
<style>
	* {
		margin: 0;
		padding: 0;
	}
	#box button.active, #box2 button.active {
		background: #ff0;
	}
	#box div, #box2 div {
		display: none;
		width: 100px;
		height: 100px;
		font-size: 20px;
		border: 1px solid #ccc;
	}
	#box div.active, #box2 div.active {
		display: block;
	}
</style>
```

```
<!-- 不自动播放 -->
<div id="box">
	<button class="active">btn1</button>
	<button>btn2</button>
	<button>btn3</button>
	<div class="active">div1</div>
	<div>div2</div>
	<div>div3</div>
</div>

<!-- 自动播放 -->
<div id="box2">
	<button class="active">btn1</button>
	<button>btn2</button>
	<button>btn3</button>
	<div class="active">div1</div>
	<div>div2</div>
	<div>div3</div>
</div>
```

```
<script>
	// 构造函数写属性
	function Tab(id) {
		if(!id) {
			return;
		}
		this.oBox = document.getElementById(id);
		this.aBtn = this.oBox.getElementsByTagName('button');
		this.aDiv = this.oBox.getElementsByTagName('div');
	}

	// 原型写方法
	Tab.prototype.fnClick = function() {
		var _this = this;
		for (var i = 0; i < this.aBtn.length; i++) {
			(function(index) {
				_this.aBtn[i].onclick = function() {
					_this._click(this, index);
				}
			})(i);
		}
	}

	Tab.prototype._click = function(oBtn, index) {
		for (var i = 0; i < this.aBtn.length; i++) {
			this.aBtn[i].className = '';
			this.aDiv[i].className = '';
		}

		oBtn.className = 'active';
		this.aDiv[index].className = 'active';
	}

	// 自动播放属性
	function AutoTab(id) {
		var _this = this;
		Tab.call(this, id);  // 继承Tab属性
		this.iNow = 0;  // 当前选项卡索引值
		this.timer = null;
		clearInterval(this.timer);
		this.timer = setInterval(function() {
			_this.next();
		}, 1000);
	}

	AutoTab.prototype = new Tab();  // 继承Tab方法
	AutoTab.prototype.constructor = AutoTab;

	// 自动播放方法
	AutoTab.prototype.next = function() {
		this._click(this.aBtn[this.iNow], this.iNow);
		this.iNow++;
		if(this.iNow == this.aBtn.length) {
			this.iNow = 0;
		}
		this.fnClick();
	}

	var old_click = AutoTab.prototype._click;

	AutoTab.prototype._click = function(oBtn, index) {
		this.iNow = index;
		old_click.apply(this, arguments);  // 调整this
	}

	var oTab = new Tab('box');  // 不自动播放
	var oAuto = new AutoTab('box2');
	oAuto.next();  // 自动播放

	console.log(oTab.constructor == Tab);	// true
	console.log(oTab instanceof AutoTab);	// false

	console.log(oAuto.constructor == AutoTab);	// true
	console.log(oAuto instanceof AutoTab);	// true
	console.log(oAuto instanceof Tab);	// true
</script>
```

## 9. 解决变量名冲突的多种方法
1. 闭包、自执行函数
	```
	<script>
		(function(){})();

		(function(){}());

		// 首部加符号防止报错，不限于~
		~function(){}();
	</script>
	```

2. 面向对象
3. 命名空间
4. 模块化
5. let(ES6)

## 10. 伪数组问题
- **DOM获取的元素是伪数组**
- **arguments是伪数组**

> 注意json属性名不用纯数字


## 11. js的冒泡(Bubbling Event)和捕获(Capture Event)的区别

> 这个问题在之前的原生篇没有写好，这边补充进来。面试题那篇中也有这题。


[js之事件冒泡和事件捕获详细介绍](http://www.jb51.net/article/42492.htm)
1. 冒泡型事件：事件按照从最特定的事件目标到最不特定的事件目标(document对象)的顺序触发。

2. 捕获型事件(event capturing)：事件从最不精确的对象(document 对象)开始触发，然后到最精确(也可以在窗口级别捕获事件，不过必须由开发人员特别指定)。

3. DOM事件流：**同时支持两种事件模型**：捕获型事件和冒泡型事件，但是，**捕获型事件先发生**。两种事件流会触及DOM中的所有对象，从document对象开始，也在document对象结束。
  DOM事件模型最独特的性质是，文本节点也触发事件(在IE中不会)。

示例
假设一个元素div，它有一个下级元素p。
```
<div>
　　<p>元素</p>
</div>
```
这两个元素都绑定了click事件，如果用户点击了p：

- 事件捕获
当你使用事件捕获时，**父级元素先触发**，子级元素后触发，即div先触发，p后触发。

- 事件冒泡
当你使用事件冒泡时，**子级元素先触发**，父级元素后触发，即p先触发，div后触发。

> addEventListener函数，它有三个参数，第三个参数若是true，则表示采用事件捕获，若是false，则表示采用事件冒泡。
IE只支持事件冒泡，不支持事件捕获。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2125655-7fd253b1a7979357.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 阻止冒泡
• 在W3c中，使用`stopPropagation()`方法
• 在IE下设置`oEvent.cancelBubble = true`；

> 在捕获的过程中stopPropagation()后，后面的冒泡过程也不会发生了。

### 阻止捕获
**阻止事件的默认行为**，例如`click <a>`后的跳转

• 在W3c中，使用`oEvent.preventDefault()`方法；
• 在IE下设置`window.event.returnValue = false;`或`return false`


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)