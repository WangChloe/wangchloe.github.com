---
title: 那些prototype那些call
date: 2019-05-13 03:27:35
categories: 前端札记
tags: [js]

---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---

原型链大法好！

<!-- more -->
### `Array.prototype.slice.call(arguments, 0)`  将具有length属性的对象转成数组



```javascript
<script>
//ES5 中的数组方法slice的底层内部实现
Array.prototype.slice=function(start,end){  
    var result = new Array(); //新数组
    var start = start || 0;
    var end = end || this.length; //this指向调用的对象，用了call之后，改变this的指向，指向传进来的对象
    for(var i=start; i<end; i++){
        result.push(this[i]);
    }
    return result;	//返回的为一个新的数组
}
</script>
```

``` javascript
<script>
    // 传入arguments类数组，调用Array.prototype.slice原型方法
    // 并用call()方法，将作用域限定在arguments中
    // 这里Array.prototype就可以理解为arguments
    // 参数0为slice()方法的第一个参数，即开始位置索引，返回整个数组。
    Array.prototype.slice.call(arguments, 0);
</script>
```

### `Object.prototype.toString.call(xxx)`  检查对象类型



![Object.prototype.toString.call](https://ws3.sinaimg.cn/large/006tKfTcgy1flaq2qoy21j309l06vjsa.jpg)

``` javascript
var InputValidation = {
    "isNumber": function (intArg) {
        return Object.prototype.toString.call(intArg) === "[object Number]";
    }
}

InputValidation.isNumber(111); // true
```

> [为什么用Object.prototype.toString.call(obj)检测对象类型?](https://www.cnblogs.com/youhong/p/6209054.html)
> toString为Object的原型方法，而Array 、Function等类型作为Object的实例，都重写了toString方法。不同的对象类型调用toString方法时，根据原型链的知识，调用的是对应的重写之后的toString方法（Function类型返回内容为函数体的字符串，Array类型返回元素组成的字符串.....），而不会去调用Object上原型toString方法（返回对象的具体类型），所以采用obj.toString()不能得到其对象类型，只能将obj转换为字符串类型；因此，在想要得到对象的具体类型时，应该调用Object上原型toString方法。


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)