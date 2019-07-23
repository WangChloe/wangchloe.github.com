---
title: 每天10个前端知识点：面向对象(上)
date: 2017-02-26 03:27:35
categories: 前端札记
tags: [js]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。



---

## 1. 面向对象

### (1) 对象组成
1. **属性(变量)**
2. **方法(函数)**

### (2) 面向对象特征
1. **封装**
2. **继承**
    子级可以继承父级的一切东西
3. **多态**
    子级可以继承多个父级

### (3) 对象相关方法

<!-- more -->

- instanceof  判断是否属于该类型
  - true 属于
  - false 不属于
  eg: `arr instanceof Array;`  // true

- constructor  查找对象的父级
  eg: `arr.constructor == Array;`  // true

> **JSON不是一个类型，其父类型就是Object**
`json.constructor == JSON;`  // false
`json.constructor == Object;`  // true

#### 执念

```
<script>
	var arr = [1, 2];
	var json = {};
	var oDate = new Date();

	console.log(arr.constructor == Array); // true
	console.log(json.constructor == JSON); // false
	console.log(json.constructor == Object); // true

	console.log(typeof oDate); // object
	console.log(oDate instanceof Date); // true
	console.log(typeof Date); // function
	console.log(Date instanceof Function); // true
	console.log(oDate instanceof Function); // false

	console.log(typeof Image); // function
	console.log(Image instanceof Function); // true

	// 开始划重点
	console.log(Function instanceof Object); // true
	console.log(Object instanceof Function); // true
	console.log(Object instanceof Object); // true
	console.log(Function instanceof Function); // true

	console.log(arr instanceof Array); // true
	console.log(Array instanceof Object); // true
	console.log(arr instanceof Object); // *true

	console.log(arr instanceof Array); // true
	console.log(Array instanceof Function); // true
	console.log(arr instanceof Function); // *false

	Object.prototype.run = 7;
	var run = 5;
	var arr2 = [];
	console.log(run); // 5
	console.log(typeof run); // number
	console.log(run instanceof Number); // false
	console.log(Number instanceof Object); // true
	console.log(Boolean instanceof Object); // true
	console.log(run instanceof Object); // false
	console.log(arr2.run); // 7
</script>
```

## 2. 引用类型
- Object类型
- Array类型
- Date类型
- RegExp类型
- Function类型

- 基本包装类型
  - Boolean类型
  - String类型
- 内置对象
  - Global对象
  - Math对象

### Array.sum实现原理

``` javascript
<script>
	var arr = [1, 2, 3];

	// sum的实现原理
	Array.prototype.sum = function() {
		var sum = 0;
		for(var i = 0; i < this.length; i++) {
			sum += this[i];
		}
		return sum;
	}

	console.log(arr.sum());
</script>
```

### Array.forEach实现原理

```
<script>
	// forEach实现原理
	if (!Array.prototype.forEach) {
		Array.prototype.forEach = function(fn) {
			for (var i = 0; i < this.length; i++) {
				fn(this[i], i, this);
			}
		};
	}

	["a", "b", "c"].forEach(function(value, index, array) {
		assert(value, "Is in position " + index + " out of " + (array.length - 1));
	});
</script>
```

### Array.every实现原理

```
<script>
	if (Array.prototype.every === undefined) {
		Array.prototype.every = function(fun) {
			//遍历当前数组中每个元素
			for (var i = 0; i < this.length; i++) {
				if (this[i] !== undefined) {
					//调用fun,依次传入当前元素值,位置i,当前数组作为参数  ，将返回值，保存在变量r中
					var r = fun(this[i], i, this);
					if (r == false) { //如果r为false
						return false; //返回false
					}
				}
			} //(遍历结束)
			return true; //返回true
		}
	}
</script>
```

### Array.some原理

```
<script>
	if (Array.prototype.some === undefined) {
		Array.prototype.some = function(fun) {
			for (var i = 0; i < this.length; i++) {
				if (this[i] !== unefined) {
					var r = fun(this[i], i, this);
					if (r == true) {
						return true;
					}
				}
			}
			return false;
		}
	}
</script>
```

### Array.map原理

```
<script>
	if (Array.prototype.map === undefined) {
		Array.prototype.map = function(fun) {
			//创建空数组: newArr
			var newArr = [];
			//遍历当前数组中每个元素
			for (var i = 0; i < this.length; i++) {
				//如果当前元素不是undefined
				if (this[i] !== undefined) { //判断稀疏数组
					//调用fun传入当前元素值，位置i，当前数组，将结果保存在r中
					//将newArr的i位置赋值为r
					var r = fun(this[i], i, this);
					newArr[i] = r;
				}
			} //(遍历结束)
			return newArr; //返回newArr
		}
	}
</script>
```

### Array.reduce原理

```
<script>
	if (Array.prototype.reduce === undefined) {
		Array.prototype.reduce = function(fun, base) {
			base === undefined && (base = 0);
			for (var i = 0; i < this.length; i++) {
				if (this[i] !== undefined) {
					base = fun(base, this[i], i, this);
				}
			}
			return base;
		}
	}
</script>
```

### Function.bind原理

```
<script>
	if (Function.prototype.bind === undefined) {
		Function.prototype.bind = function(obj /*，参数列表*/ ) {
			var fun = this; //留住this
			//*****将类数组对象，转化为普通数组
			var args = Array.prototype.slice.call(arguments, 1);
			//args保存的就是提前绑定的参数列表
			/*function slice(1){
			   var sub=[];
			   for(var i=0;i<length;i++){
			    sub.push(arguments[i]);
			   }
			   return sub;
			}*/
			return function() {
				//将后传入的参数值，转为普通数组      
				var innerArgs = Array.prototype.slice.call(arguments); //将之前绑定的参数值和新传入的参数值，拼接为完整参数之列表
				var allArgs = args.concat(innerArgs)
					//调用原始函数fun，替换this为obj，传入所有参数
				fun.apply(obj, allArgs);
			}
		}
	}
</script>
```

## 3. 创建对象前导
### (1) 关于new
1. 创建一个空对象，并赋值给this
2. 返回this

### (2) 关于this
当前方法属于谁，this就是谁

```
<script>
	// In web browsers, the window object is also the global object:
	console.log(this === window); // true
</script>
```

**this的优先级**：
1. new -> object
2. 定时器 -> window
3. 事件 -> 事件对象
4. 方法 -> 方法对象

> **多包一层时优先级失效**

哈哈，关于this的坑详见后面的坑集合！

```
<script>
	function show() {
		console.log(this);
	}

	var arr = [1, 2];
	arr.show = show;

	document.onclick = arr.show; // 点击时 document
	new arr.show(); // 1. object
	new show(); // 2. object
	new document.onclick(); // 3. object

	setTimeout(show, 1000); // 9. window
	setTimeout(arr.show, 1000); // 10. window

	setTimeout(new arr.show, 1000); // 4. object **在上两个行执行前先弹出

	var oDate = new Date();
	oDate.show = show;
	document.show = show;
	document.onclick = show;
	setTimeout(function() { // setTimeout多包一层优先级失效
		oDate.show(); // 5. oDate时间
		new oDate.show(); // 6. object
		document.show(); // 7. document
		document.onclick(); // 8. document
	}, 100);
</script>
```

#### 强制改变this指向
- call
  - `fn.call(a);`  改变this指向，指向a
  - `fn.call(a, p1, p2);`  改变this指向并传参p1, p2
- apply
  - `fn.apply(a, [p1, p2]);`  改变this指向并传入参数数组
  - `fn.apply(a, arguments);`  改变this指向并传入当前方法(非fn)的参数数组


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)