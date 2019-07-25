---
title: 每天10个前端知识点：杂技
date: 2019-07-20 03:27:35
categories: 前端札记
tags: [杂技]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

<!-- MarkdownTOC -->

- toString\(\) && valueOf\(\)
- JSON stringify & parse
- zepto & jquery区别
- `xxx.prepend(yyy)`会先删除yyy再移入xxx最前
- format传入参数实现
- `+new Date()`
- `Array.prototype.slice.call(arguments, 0)`剖析
- `Object.prototype.toString.call(xxx);`
- 数组或对象的长度
- 数组循环
- 微信关闭当前页面
- 强制刷新
- 正则获取指定key的value
- chrome清除缓存、host
- localStorage判断再次访问是否是当天

<!-- /MarkdownTOC -->


<!-- more -->

---

有些平时碰到的很零碎的东西我就随便插入到这个章节里了。

## toString() && valueOf()

- `toString()` 把一个逻辑值转换为字符串，并返回结果。

- `valueOf()`  返回Boolean对象的原始值

> **源自知乎**
这两个方法一般是交由JS去隐式调用，以满足不同的运算情况。

> 在数值运算里，会优先调用valueOf()，如a+b；
在字符串运算里，会优先调用toString()，如alert(c)。

``` javascript
    // 该示例来源于脚本之家
    var bbb = {
        i: 10,
        toString: function() {
            console.log('toString');
            return this.i;
        },
        valueOf: function() {
            console.log('valueOf');
            return this.i;
        }
    }

    alert(bbb); // 10 toString
    alert(+bbb); // 10 valueOf
    alert('' + bbb); // 10 valueOf
    alert(String(bbb)); // 10 toString
    alert(Number(bbb)); // 10 valueOf
    alert(bbb == '10'); // true valueOf
    alert(bbb === '10'); // false
```

``` javascript
    console.log({
        valueOf: function() {
            return 20;
        }
    } * {
        valueOf: function() {
            return 30;
        }
    });     // 600
```

## JSON stringify & parse

[json2.js - 引入解决IE7及以下版本JSON未定义问题。](https://github.com/douglascrockford/JSON-js/blob/master/json2.js)

1. JSON.stringify(object);  **对象 -> 字符串**  将对象字符串序列化成标准JSON字符串

eg: `{a:1,b:2}`  ->  `"{"a":1,"b":2}"`

2. JSON.parse(str);  **字符串 -> json对象**  将字符串序列化成对象

`{"name":"wangchloe","age":"22"}`  ->
```
{
    age: "22",
    name: "wangchloe",
    _proto: Object
}
```

``` html
<a href="https://www.baidu.com/" attr1='13'>baidu.com</a>
```

``` javascript
var oA = document.querySelector('a');
console.log(oA.getAttribute('attr1'));  // 13

oA.setAttribute('attr1', '14');
var num = oA.getAttribute('attr1');

console.log(oA.getAttribute('attr1'));  // 14
console.log(typeof number);  // string 直接设置自定义属性只能得到string类型

oA.setAttribute('attr1', JSON.stringify({name: 14}));

var num2 = oA.getAttribute('attr1');

console.log(num2);  // {"name": "14"}
console.log(JSON.parse(num2));
// Object {name: "14"}
//     name: "14"
//     -> _proto_: Object

console.log(JSON.parse(num2).name);  // 14
console.log(typeof JSON.parse(num2).name);  // number JSON转化得到了真正类型
```

3. JSON转换 `JSON.parse(JSON.stringify(obj))`

## zepto & jquery区别
  - zepto没有判断`is(":hidden")`方法
  - zepto没有`fadeIn`、`fadeOut`方法 
  - jquery中`visibility:hidden`、 `opacity:0`为可见


## `xxx.prepend(yyy)`会先删除yyy再移入xxx最前


## format传入参数实现

"//webapp/js/base.js",
``` javascript
    String.prototype.format = function () {
        for (var temS = this, i = 0; i < arguments.length; ++i) {
            temS = temS.replace(new RegExp("\\{" + i + "\\}", "g"), arguments[i]);
        }
        return temS;
    }
```

## `+new Date()`

``` javascript
    s = new Date().toString();
    // "Wed May 17 2017 11:00:16 GMT+0800 (中国标准时间)"

    t = (+new Date()).toString();
    // "1494990039861"
    // +new Date(); 等同于 new Date().getTime(); 简略写法，得到毫秒
```


## `Array.prototype.slice.call(arguments, 0)`剖析

将具有length属性的对象转成数组

```javascript
// slice的内部实现
Array.prototype.slice=function(start,end){  //ES5 中的数组方法slice的底层内部实现
    var result = new Array(); //新数组
    var start = start || 0;
    var end = end || this.length; //this指向调用的对象，用了call之后，改变this的指向，指向传进来的对象
    for(var i=start; i<end; i++){
        result.push(this[i]);
    }
    return result;  //返回的为一个新的数组
}
```

``` javascript
    // 传入arguments类数组，调用Array.prototype.slice原型方法
    // 并用call()方法，将作用域限定在arguments中
    // 这里Array.prototype就可以理解为arguments
    // 参数0为slice()方法的第一个参数，即开始位置索引，返回整个数组。
    Array.prototype.slice.call(arguments, 0);
```


## `Object.prototype.toString.call(xxx);`

![Object.prototype.toString.call](https://ws3.sinaimg.cn/large/006tKfTcgy1flaq2qoy21j309l06vjsa.jpg)

``` javascript
var InputValidation = {
    "isNumber": function (intArg) {
        return Object.prototype.toString.call(intArg) === "[object Number]";
    }
}

InputValidation.isNumber(111); // true
```

## 数组或对象的长度

``` javascript
 codeAccount: function() {
    if (Object.prototype.isPrototypeOf(this.arrCodes)) {
        var keyArray = Object.keys(this.arrCodes);
        return keyArray.length;
    } else if (Array.prototype.isPrototypeOf(this.arrCodes)) {
        returnthis.arrCodes.length;
    } else {
        return 0;
    }
}
 ```
 
## 数组循环
 
- jq元素数组each循环

``` javascript
$selector.each(function(index, ele){
    
});
```

- 纯数组each循环

``` javascript
var arr = ['a', 'b', 'c']

$.each(arr, function(index, ele) {
   console.log(ele);
});
```


## 微信关闭当前页面
`WeixinJSBridge.call('closeWindow');`

## 强制刷新

`window.location.href = window.location.href + '&t=' + new Date().getTime();`

## 正则获取指定key的value
``` javascript
var className = $selectedLottery[0].className;
var index = className ? /lottery-unit-(\d+)/.exec(className)[1] : '';
```

## chrome清除缓存、host
`chrome://net-internals/#sockets`

## localStorage判断再次访问是否是当天

``` javascript
toastNews: function() {
    var nowDate = new Date();
    // Thu Aug 23 2018 17:29:16 GMT+0800 (中国标准时间)
    
    var that = this;
    // 将上次访问时间放在localstorage中
    //取上次进入时间，判断如果是同一天的话，不提示，如果是空或者不是同一天则提示
    var lastEntryTime = localStorage.getItem('ls-time');
    
    if (!lastEntryTime || this.getDayTimestamp(lastEntryTime) != this.getDayTimestamp(nowDate)) { //空不存在或者不是同一天
        localStorage.setItem('ls-time', nowDate);
        // ...
    } else {
        // ...
    }
},
getDayTimestamp: function(date) {
    var parseDate = new Date(date);
    var year = parseDate.getFullYear();
    // new Date('Thu Aug 23 2018 17:29:16 GMT+0800 (中国标准时间)').getFullYear()
    // 2018
    
    var month = parseDate.getMonth();
    var day = parseDate.getDate();

    return new Date(year,month,day).getTime();
}
```


---

更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！

公众号是坚持日更的，不发文的时候推送一些我觉得好用的前端网站或者看到的一些问题的解决方案，也更便于大家交流，就酱。

![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)