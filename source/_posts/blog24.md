---
title: 每天10个前端知识点：面向对象(中)
date: 2017-02-27 03:27:35
categories: 前端札记
tags: [js]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

{% aplayer "Oceans Deep" "Sons Of Day" "http://ojvx9eehr.bkt.clouddn.com/music/Sons%20Of%20Day%20-%20Oceans%20Deep.mp3" %}

---

## 4. 创建对象

这里先提一下**23种设计模式**：

- 创建型模式(5种)
  - 工厂方法模式
  - 抽象工厂模式
  - 单例模式
  - 建造者模式
  - 原型模式

<!-- more -->

- 结构型模式(7种)
  - 适配器模式
  - 装饰器模式
  - 代理模式
  - 外观模式
  - 桥接模式
  - 组合模式
  - 享元模式

- 行为型模式(11种)
  - 策略模式
  - 模板方法模式
  - 观察者模式
  - 迭代子模式
  - 责任链模式
  - 命令模式
  - 备忘录模式
  - 状态模式
  - 访问者模式
  - 中介者模式
  - 解释器模式

### (1) 工厂模式

```
<script>
	function createObject(name, age) {
		// 1.原料
		var obj = new Object();  // 创建自定义对象

		// 2.加工
		// 属性
		obj.name = name;
		obj.age = age;
		// 方法
		obj.getName = function() {
			return this.name;
		}
		obj.getAge = function() {
			return this.age;
		}

		// 3.出厂
		return obj;
	}

	var p = createObject('x', 11);
	console.log(p.getName());
</script>
```

### (2) 单例模式
**需要重新指定父级**

> 这个例子有点问题，等我找到合适的补上，或者评论区留言给我。


```
<script>
	var person = {};

	// 加属性
	person.name = 'xxx';

	// 加方法
	person.getName = function() {
		return this.name;
	};
</script>
```

[javascript 单例模式 - 长江之水向西流 - 博客频道 - CSDN.NET](http://blog.csdn.net/bluestarjava/article/details/9967527)


```
<script>
	function Person() {
		//缓存实例  
		var instance = this;

		//重写构造函数  
		Person = function() {
			return instance;
		}

		//保留原型属性  
		Person.prototype = this;

		//实例  
		instance = new Person();

		//重置构造函数引用  
		instance.constructor = Person;

		//其他初始化  
		instance.createTime = new Date();

		return instance;
	}
</script>
```


### (3) 构造函数模式

**构造函数函数名首字母大写** -> 区分普通函数

> 以这种方式定义的构造函数是定义在Global对象(浏览器中为window对象)中的。

```
<script>
	function CreateObject(name, age) {
		// 1.原料
		// var obj = new Object();  // 创建自定义对象

		// 2.加工
		// 属性
		// obj.name = name;
		// obj.age = age;
		this.name = name;
		this.age = age;

		// 方法
		// obj.getName = function() {
		// 	return this.name;
		// }
		// obj.getAge = function() {
		// 	return this.age;
		// }
		this.getName = function() {
			return this.name;
		}
		this.getAge = function() {
			return this.age;
		}

		// 3.出厂
		// return obj;
	}

	var p = new CreateObject('x', 11);  // 创建对象，并赋值给this
	console.log(p.getName());
	console.log(p.constructor == CreateObject); // true
	console.log(p instanceof Object); // true
	console.log(p instanceof CreateObject); // true
</script>
```

### (4) 原型模式

```
<script>
	function CreateObject() {

	}

	CreateObject.prototype.name = 'xxx';
	CreateObject.prototype.age = '11';
	CreateObject.prototype.getName = function() {
		return this.name;
	}

	var p = new CreateObject();
	console.log(p.getName());
	// 查找过程
	// 先在 p上查找有没有getName属性
	// 没有，再在p的原型上查找getName属性

	console.log(CreateObject.prototype.isPrototypeOf(p));  // true

	console.log(p.hasOwnProperty('name'));  // false  原型属性
	p.name = 'yyy';
	console.log(p.hasOwnProperty('name'));  // true  实例属性
	delete p.name;  // 删除p.name
	console.log(p.hasOwnProperty('name'));  // false  原型属性

	console.log('name' in  p);  // true
</script>
```

#### 重写prototype将导致实例的constructor转向

```
<script>
	function CreateObject() {

	}

	// CreateObject.prototype.name = 'xxx';
	// CreateObject.prototype.age = '11';
	// CreateObject.prototype.getName = function() {
	// 	return this.name;
	// }
	CreateObject.prototype = {
		// constructor: CreateObject,  // 可特意声明constructor指向 CreateObject
		name: 'xxx',
		age: '11',
		getName: function() {
			return this.name;
		}
	}

	var p = new CreateObject();
	console.log(p.getName());

	console.log(p instanceof CreateObject);  // true
	console.log(p instanceof Object);  // true
	console.log(p.constructor == CreateObject);  // false

	// 重写默认的prototype后，constructor不在指向CreateObject，而是指向Object
	console.log(p.constructor == Object);  // true
</script>
```

### (5) 构造+原型

原型模式的问题

```
<script>
	function CreateObject() {

	}

	CreateObject.prototype = {
		constructor: CreateObject,  // 可特意声明constructor指向 CreateObject
		name: 'xxx',
		age: '11',
		children: ['aaa', 'bbb'],
		getName: function() {
			return this.name;
		}
	}

	var p = new CreateObject();
	var p2 = new CreateObject();

	p.children.push('ccc');

	console.log(p.children); // 'aaa,bbb,ccc'
	console.log(p2.children); // 'aaa,bbb,ccc'
	console.log(p.children == p2.children);  // true
	// 尴尬了，p新增加的孩子同时增加到了p2上
</script>
```

改为 构造+原型

> 1. 构造定义实例属性
2. 原型定义方法及共享属性

```
<script>
	function CreateObject(name, age) {
			this.name = name;  // 实例属性
			this.age = age;  // 实例属性

			this.children = ['aaa', 'bbb'];  // 实例属性
	}

	CreateObject.prototype = {
		constructor: CreateObject,  // 共享属性
		getName: function() {  // 共享方法
			return this.name;
		}
	}

	var p = new CreateObject();
	var p2 = new CreateObject();

	p.children.push('ccc');

	console.log(p.children); // 'aaa,bbb,ccc'
	console.log(p2.children); // 'aaa,bbb'
	console.log(p.children == p2.children);  // false
	console.log(p.getName == p2.getName);  // true
</script>
```

## 5. 原型与原型链
### 关于原型
> 
1. 对象有属性**`_proto__`**(隐式原型)，指向该对象的构造函数的原型对象。**（不要忘记万物皆对象）**
2. 方法(也是对象)除了有属性**`__proto__`**，还有属性**`prototype`**，prototype指向该方法的原型对象。
3. 原型对象还有属性**`constructor`**，指向原构造函数。

### 关于原型链

- 只要创建一个新函数(CreateObject)，都会附带一个prototype属性，并指向函数的原型对象(CreateObject.prototype)。
  `CreateObject.prototype` -> `CreateObject.prototype`

- 所有原型对象(CreateObject.prototype)都附带一个constructor(构造函数)属性，并指向prototype属性所在函数的指针(CreateObject)。
  `CreateObject.prototype.constructor` -> `CreateObject`

- 调用构造函数创建的实例(p)，内部将包含指针_proto_，并指向构造函数的原型对象(CreateObject.prototype)。
  `p._proto_` -> `CreateObject.prototype`
  `CreateObject._proto_` -> `Function.prototype`
  `Function.prototype._proto_` -> `Object.prototype`
  `CreateObject.prototype._proto_` -> `Object.prototype`
  `Object.prototype._proto_` -> `null`

![_proto_ && prototype && constructor](http://images0.cnblogs.com/blog2015/683809/201508/201728184569708.jpg)

## 6. 原型应用
### (1) 数组arr.indexOf()兼容问题
*兼容：高级浏览器
IE8- -> 报错

兼容写法
```
<script>
	var arr = [1, 2, 3, 4];

	Array.prototype.indexOf = Array.prototype.indexOf || function(item) {  // 如果有indexOf()就用系统自带的
		// IE8-
		for (var i = 0; i < this.length; i++) {
			if(this[i] == item) {
				return i;
			}
		}

	}

	console.log(arr.indexOf(3));
</script>
```

### (2) 字符串str.trim()兼容问题
*兼容：高级浏览器
IE8- -> 报错

兼容写法
```
<script>
	var str = ' aaa ';

	String.prototype.indexOf = String.prototype.indexOf || function() {  // 如果有trim()就用系统自带的
		// IE8-
		return this.replace(/^\s+|\s+$/g, '');
	}

	console.log('去空格' + str.trim() + '去空格');
</script>
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)