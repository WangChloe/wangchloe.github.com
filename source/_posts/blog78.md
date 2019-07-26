---
title: es6笔记
categories: 前端札记
tags: js
date: 2019-06-01 17:51:06
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

<!-- MarkdownTOC -->

- 数据类型
	- Symbol
- 数据结构
	- Map
		- WeakMap
	- Set
		- WeakSet
- 声明变量
	- let
		- let声明的变量只在当前代码块内有效
		- for循环变量为副作用域，循环内部为子作用域，let范围不冲突不交叉
			- 字符串遍历器
		- 不存在变量提升
		- 暂时性死区
		- 不允许重复声明
	- const
		- 应用场景
	- ES5 & ES6 声明变量
		- 顶层对象 & 全局变量
	- global对象
		- 顶层对象
		- this变量
		- 引入global作为顶层对象
- destructuring 解构赋值
	- 数组的解构赋值
	- 默认值
	- 对象的解构赋值
	- 应用
- class类
- arrow function 箭头函数
- \`\` template string 模版字符串
- Spread 扩展运算符 ...
- default reset
	- default
		- reset
- import export
	- es5
	- es6
- proxy & reflect
	- Proxy
	- Reflect
- Promise
- for...of循环

<!-- /MarkdownTOC -->

<!-- more -->


[学会了ES6，就不会写出那样的代码](https://mp.weixin.qq.com/s/-INIDCq19yl8nKWHXYKZHg)

# 数据类型

## Symbol

- 独一无二的值

``` javascript
let a1 = Symbol();
let a2 = Sybmol();

console.log(a1 === a2); //false

let a3 = Sybmol.for('a3');
let a4 = Sybmol.for('a3');

console.log(a3 === a4); // true
console.log(typeof Symbol); // function
console.log(Symbol.toString()); // "function Symbol() { [native code] }"
```

# 数据结构

## Map

>  键值对集合，键的范围不限于字符串

- `map.size`

- `map.has('xxx')`

- `map.get('xxx')`

- `map.set(arr, 'xxx')`

- `map.delete('xxx')`

- `map.clear()`

``` javascript
const map = new Map([
  ['name', 'Chloe']，
  ['title', 'Author']
  ]);

map.size // 2
map.has('name') // true
map.get('name') // Chloe
map.has('title') // true
map.get('title') // Author

let map2 = new Map();
let arr = ['123'];
map2.set(arr, '456');
console.log(map2, map2.get(arr)); // {["123"] => "456"}  "456"
```

### WeakMap

- WeakMap只接受对象作为键名

- 没有clear方法

## Set

- 类似数组，但是成员的值都是唯一的，没有重复的值。

``` javascript
let arr1 = [1, 2, 3 ,1];

let list3 = new Set(arr1);

console.log(list3); // {1, 2, 3, 4}
console.log(list3.size); // 4

console.log(typeof Set); // function
console.log(Set.toString()); // "function Set() { [native code] }"
```

- `list.size`

- `list.add(xxx)` 添加值，返回Set解构本身

- `list.delete(xxx)` 删除值，返回是否删除成功的boolean

- `list.has(xxx)` 该值是否为Set的成员，返回一个boolean

- `list.clear()` 清除所有成员，没有返回值


- 遍历

``` javascript
let arr = ['add', 'delete', 'clear'];
let list = new Set(arr);

for(let key of lists.leys()) {
  console.log(key);
}

list.forEach(function(item) {
  console.log(item)
})
```

### WeakSet

- WeakSet成员只能是对象

- 没有clear方法

# 声明变量

## let

let & var

### let声明的变量只在当前代码块内有效

``` javascript
{
    let a = 10;
    var b = 1;
}

a  // a is not defined
b  // 1

```

- eg: 可用于for循环计数

``` javascript
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
```

``` javascript
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6
```

### for循环变量为副作用域，循环内部为子作用域，let范围不冲突不交叉

``` javascript
for(let i = 0; i < 3; i++) {
    let i = 'abc';
    console.log(i);
}

//abc
//abc
//abc
```

#### 字符串遍历器

``` javascript
let str = 'abc';

for(let code of str) {
  console.log('es6', code); // abc
}
```

### 不存在变量提升

``` javascript
console.log(foo); // undefined
var foo = 2;

console.log(bar); // 报错
let bar = 2;
```

### 暂时性死区

``` javascript
var tmp = 123;

if(true) {
    tmp = 'abc'; // 报错
    let tmp;
}
```

let绑定了if语句的块级作用域

``` javascript
var x = x; // 不报错

let x = x; // 报错
```

### 不允许重复声明

``` javascript
// 报错
function func() {
    let a = 10;
    var a = 1;
}

// 报错
function func() {
    let a = 10;
    let a = 1;
}
```

``` javascript
function func(arg) {
    let arg; // 报错
}

function func(arg) {
    {
        let arg;
    }
}
```

> 函数声明

- 允许在块级作用域内声明函数。

- 函数声明类似于var，即会提升到全局作用域或函数作用域的头部。

- 同时，函数声明还会提升到所在的块级作用域的头部。

``` javascript
// *浏览器的 ES6 环境
function f() { console.log('I am outside!'); }

(function () {
  // var f = undefined;

  if (false) {
    // 重复声明一次函数f
    function f() { console.log('I am inside!'); }
  }

  f();  // f is not a function
}());
```

**优化 => 推荐函数表达式**

``` javascript
// 函数声明语句
{
  let a = 'secret';
  function f() {
    return a;
  }
}

// 函数表达式
{
  let a = 'secret';
  let f = function () {
    return a;
  };
}
```

## const

- 声明常量

- 只在声明所在的块级作用域内有效

> const并不是变量的值不得改动，而且是变量指向的**内存地址**不得改动。


简单类型的数据(数值、字符串、布尔值) => 常量

复合类型的数据(对象、数组等) => 指针地址

``` javascript
const foo = {};

// foo可以添加自身属性
foo.prop = 123;
foo.prop;  // 123

foo = {};  // 报错，foo不能指向另一个对象

const a = [];
a.push('Hello'); // 可添加数据
a.length = 0;  // 可定义属性

a = ['Chloe'];  // 报错，不能指向另一个数组
```

### 应用场景

引用第三方库的时声明的变量，用const来声明可以避免未来不小心重命名而导致出现bug。

``` javascript
const monent = require('moment');
```

## ES5 & ES6 声明变量

- ES5: `var` `function`

- ES6: `var` `function` `let` `const` `import` `class`


### 顶层对象 & 全局变量

- var命令和function命令声明的全局变量，依旧是顶层对象的属性

- let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性

``` javascript
var a = 1;
window.a; // 1

let b = 1;
window.b; // undefined
```

## global对象

### 顶层对象

- 浏览器里面，顶层对象是window，但 Node 和 Web Worker 没有window。

- 浏览器和 Web Worker 里面，self也指向顶层对象，但是 Node 没有self。

- Node 里面，顶层对象是global，但其他环境都不支持。

### this变量

- 全局环境中，this会返回顶层对象。但是，Node 模块和 ES6 模块中，this返回的是当前模块。

- 函数里面的this，如果函数不是作为对象的方法运行，而是单纯作为函数运行，this会指向顶层对象。但是，严格模式下，这时this会返回undefined。

- 不管是严格模式，还是普通模式，new Function('return this')()，总是会返回全局对象。但是，如果浏览器用了 CSP（Content Security Policy，内容安全政策），那么eval、new Function这些方法都可能无法使用。


### 引入global作为顶层对象

- 方法一

``` javascript
(typeof window !== 'undefined'
    ? widnow
    : (typeof process === 'object' &&
       typeof require === 'function' &&
       typeof global === 'object')
    ? global
    : this);
```

- 方法二

``` javascript
var getGlobal = function () {
    if(typeof self !== 'undefined') { return self; }
    if(typeof window !== 'undefined') { return window; }
    if(typeof global !== 'undefined') {return global; }
    throw new Error('unable to locate global object');
};
```

# destructuring 解构赋值

从数组和对象中提取值，对变量进行赋值。


## 数组的解构赋值

``` javascript
let [a, b, c] = [1, 2, 3];

let [x, , y] = [1, 2, 3];
x // 1
y // 3

let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]


let [x, ,y, ...z] = ['a'];
x // "a"
y // undefined
z // []

let [a, [b], [d]] = [1, [2, 3], 4];
a // 1
b // 2
d // 4
```

## 默认值

> ES6 内部使用严格相等运算符（===），判断一个位置是否有值。所以，只有当一个数组成员严格等于undefined，默认值才会生效。


``` javascript
let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'

let [x = 1] = [undefined]; // x=1

let [x = 1] = [null]; // x=null
```



## 对象的解构赋值

> 对象的属性没有次序，变量必须与属性同名，才能取到正确的值。

``` javascript
let { bar, foo } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"

let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
console.log(baz); // "aaa"
console.log(foo); // foo is not defined 
```

> 解构赋值的内部机制：先找到同名属性，然后再赋给对应的变量。真正被赋值的是baz而不是foo。


``` javascript
// es5
let cat = 'ken'
let dog = 'lili'
let zoo = {cat: cat, dog: dog}  // {cat: 'ken', dog: 'lili'}

// es6
let cat = 'ken'
let dog = 'lili'
let zoo = {cat, dog}; // {cat: 'ken', dog: 'lili'}
```

## 应用

- 变量交换

``` javascript
{
  let a = 1;
  let b = 2;
  [a, b] = [b, a];
  console.log(a, b); // 2 1
}
```

# class类

- extends

- super()

``` javascript
class Animal {
  // 构造方法
    constructor(){
        this.type = 'animal'
    }
    says(say){
        console.log(this.type + ' says ' + say)
    }
}

let animal = new Animal()
animal.says('hello') //animal says hello

// 继承
class Cat extends Animal {
    constructor(){
        // 父类的实例方法，继承父类的this对象，不在constructor中使用this则不必调用super()
        super()
        this.type = 'cat'
    }
}

let cat = new Cat()
cat.says('hello'); //cat says hello
```


# arrow function 箭头函数

``` javascript
// es5
function(i) {
  return i + 1;
}

// es6
(i) => i + 1

// es5
function(x, y) {
  x++;
  y--;
  return x + y;
}

// es6
(x, y) => { x++; y--; return x+y}

// es5
var foo = function(a, b) {
  return a * b;
}

// es6
let bar = (a, b) => a * b;

// es5
var fn = function () {
    return new Date().getFullYear() - this.birth; // this指向window或undefined
};

// es6
var fn = () => new Date().getFullYear() - this.birth; // this指向obj对象

// es6  【和函数体的{ ... }有语法冲突，需包裹小括号】
x => ({ foo: x })

// es6 由于this在箭头函数中已经按照词法作用域绑定了，所以，用call()或者apply()调用箭头函数时，无法对this进行绑定，即传入的第一个参数被忽略
var obj = {
    birth: 1990,
    getAge: function (year) {
        var b = this.birth; // 1990
        var fn = (y) => y - this.birth; // this.birth仍是1990
        return fn.call({birth:2000}, year);
    }
};
obj.getAge(2015); // 25
```

- 数组应用

``` javascript
let arr = [ 5, 6, 7, 8, 'a' ];
let b = arr.map( item => item + 3 );
console.log(b); // [ 8, 9, 10, 11, 'a3' ]
```

- 处理this

> 原理：箭头函数没有自己的this，他的this是继承外面的，因此内部的this就是外层代码块的this。

``` javascript
class Animal {
    constructor(){
        this.type = 'animal'
    }
    says(say){
        setTimeout( () => {
            console.log(this.type + ' says ' + say)
        }, 1000)
    }
}

var animal = new Animal()
animal.says('hi');  //animal says hi
```


# \`\` template string 模版字符串

- \` (反引号)模版字符串 包裹字符串及变量

- `${xxx}` 引用变量

``` javascript
var fName = 'Peter', sName = 'Smith', age = 43, job = 'photographer';
var a = 'Hi, I\'m ' + fName + ' ' + sName + ', I\'m ' + age + ' and work as a ' + job + '.';
var b = `Hi, I'm ${ fName } ${ sName }, I'm ${ age } and work as a ${ job }.`;

$("#result").append(`
  There are <b>${basket.count}</b> items
   in your basket, <em>${basket.onSale}</em>
  are on sale!
`);
```

# Spread 扩展运算符 ...


- 将多个参数合并到一个数组中

``` javascript
console.log(...[1,2,3]); // 1 2 3

let a = [3, 4, 5];
let b = [1, 2, ...a, 6];
console.log(b);  // [1, 2, 3, 4, 5, 6]

function foo(a, b, c) { console.log(`a=${a}, b=${b}, c=${c}`)}
let data = [5, 15, 2];
foo( ...data); // a=5, b=15, c=2
```

- 将数组或对象分散到新的数组或对象中

``` javascript
let a = [1, 2, 3];
let b = [ ...a ];
let c = a;
b.push(4);
console.log(a);  // [1, 2, 3]
console.log(b);  // [1, 2, 3, 4] referencing different arrays
c.push(5);
console.log(a);  // [1, 2, 3, 5]
console.log(c);  // [1, 2, 3, 5] referencing the same array
```
 
# default reset

## default

``` javascript
// es5
function animal(type) {
  type = type || 'cat'
  console.log(type)
}

// es6
function animal(type = 'cat') {
  console.log(type);
}
```

### reset

- `...变量名` 获取函数的多余参数

- 参数变量是一个数组，该变量将多余的参数放入数组中

``` javascript
function animals(...types) {
  console.log(type)
}

animals('cat', 'dog', 'fish'); // ["cat", "dog", "fish"]
```

# import export

- es5 
  - 服务器端 CommonJS
  - 浏览器端 AMD(eg:require.js)

- es6
  - module功能

## es5

- require.js

``` javascript
// content.js
define('content.js', function() {
  return 'A cat'
  })

// index.js
require(['./content.js'], function(animal) {
  console.log(animal); // A cat
  });
```

- CommonJS

``` javascript
// index.js
var animal = require('./content.js')

// content.js
module.exports = 'A cat'
```

## es6

``` javascript
// index.js
import animal from './content'

// content.js
export default 'A cat'
```

# proxy & reflect

## Proxy

``` javascript
let obj = {
  name:'gao',
  time:'2017-08-13',
  emp:'123'
}

let temp = new Proxy(obj, {
  get(target, key) {
    return target[key].replace('2017', '2018');
  }, {
  set(target, key, value) {
    if(key === 'name') {
      return target[key] = value;
    } else {
      return target[key];
    }
  },
  deleteProperty(target, key) {
    if(key.indexOf('i') > -1) {
      delete target[key];
      return true;
    } else {
      return target[key];
    }
  },
  ownKeys(target) {
    return Object.keys(target).filter(item=>item!='name');
  }
})

console.log('get',temp.time);  //get 2018-08-13

temp.time = '2018';
console.log('set',temp.name,temp); //set gao   {name: "gao", time: "2017-08-13", temp: "123"}

temp.name = 'he';
console.log('set',temp.name,temp); // set he  {name: "he", time: "2017-08-13", temp: "123"}

console.log('has','name' in temp,'time' in temp);  //has true false

delete temp.time;
console.log('delete',temp);   //delete  {name: "he", temp: "123"}
```

## Reflect

> 不管Proxy怎么修改默认行为，总可以在Reflect上获取默认行为。


``` javascript
let obj = {
  name:'gao',
  time:'2017-08-13',
  emp:'123',
}

console.log('reflect get',Reflect.get(obj, 'name'));  // reflect get gao
Reflect.set(obj,'name','hexaiofei');
console.log(obj);  // {name: "hexaiofei", time: "2017-08-13", emp: "123"}
console.log('reflect has', Reflect.has(obj,'name'));  //reflect has true

console.log(typeof Reflect); // "object"
console.log(Reflect.ownKeys.toString()); // "function ownKeys() { [native code] }"
```

# Promise

``` javascript
console.log(typeof Promise); //"function"
console.log(Promise.toString()); //"function Promise() { [native code] }"
```

>  异步编程解决方案

- 三种状态
  - Pending(进行中)
  - Fulfilled(已成功)
  - Rejected(已失败)

- Resolved(已定型)
  - Pending -> Fulfilled
  - Pending -> Rejected

- Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve(必写)和reject(可选)
  - resolve (Pending -> Resolved) 未完成->成功
  - reject (Pending -> Rejected) 未完成->失败

> Promise实例生成以后，可以用then方法分别指定Resolved状态和Rejected状态的回调函数。

``` 
// ES5回调函数
let ajax = function(callback) {
  console.log('hello');
  setTimeout(funciton(){
    callback && callback()
    }, 1000)
}

ajax(function(){
  console.log('timeout1');
  })

// ES6 Promise
let ajax = function() {
  console.log('say');
  return new Promise((resolve, reject) => {
    setTimeout(function(){
      resolve();
    }, 1000);
  });
}

// Promise实例生成以后，可以用then方法分别指定Resolved状态和Rejected状态的回调函数。
ajax().then(function() {
  console.log('promise', 'timeout1');
})

```

- promise用法

``` javascript
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```

- promise对象回调处理详解

``` javascript
console.log('我是顺序运行的1号');

//回调测试1，带promise实例化时的参数
function test1(value){
    console.log('回调测试1')
    return value
}

//回调测试2，不带promise实例化时的参数
function test2(){
    console.log('回调测试2')
    return arguments[0]
}

//创造了一个Promise实例
let p = new Promise(function (resolve, reject) {
    console.log('创造了一个Promise实例');
    //异步操作成功时调用，并将异步操作的结果，作为参数传递出去
    resolve(666);
});
console.log('我是顺序运行的2号');


//方法一：最简便的,拿到的是创造Promise实例时给resolve的参数
p.then(function(result) {
  // success
  console.log('方法一回调成功结果是:'+result)
}, function(error) {
  // failure
  console.log('failure1:'+error)
});

//方法二：先执行指定的函数再回调，test1这时就是resovle,所以参数是666
p.then(test1)
 .then(function (result) {
    console.log('方法二回调成功结果是:' + result);
}, function(error) {
  // failure
  console.log('failure2:'+error)
});

//方法三：先执行指定的函数再回调，test1这时就是resovle,所以默认参数是arguments里面也有666
p.then(test2)
 .then(function (result) {
    console.log('方法三回调成功结果是:' + result);
}, function(error) {
  // failure
  console.log('failure3:'+error)
});
console.log('我是顺序运行的3号');
```

![](https://img-blog.csdn.net/20180309104556172?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTQxOTQxOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


# for...of循环

``` javascript
let a = ['a', 'b', 'c', 'd'];

for(var val of a) {
  console.log(val);
}
// "a" "b" "c" "d"
```

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)