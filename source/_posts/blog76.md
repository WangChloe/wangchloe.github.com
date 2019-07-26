---
title: Array高阶函数
categories: 前端札记
tags: js
date: 2019-02-26 16:46:58
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---
<!-- MarkdownTOC -->

- filter
- map
- reduce
	- 计算重复单词
	- array.reduce\(fn, value\)
		- 累加
		- 初始值的重要性
- sort
	- 添加函数
- every
- forEach
- $.inArray\(\)
- 应用

<!-- /MarkdownTOC -->


<!-- more -->

[ES6---数组array新增方法](https://blog.csdn.net/wbiokr/article/details/65939582)

[5个数组Array方法: indexOf、filter、forEach、map、reduce使用实例](https://www.jb51.net/article/60502.htm)

[【译】高阶函数：利用Filter、Map和Reduce来编写更易维护的代码](https://zhuanlan.zhihu.com/p/28835709)

- forEach没有返回值，重点是function里面处理逻辑
- map有返回值，重点是function返回值，组成新数组
- filter有返回值，重点是function返回值，过滤之后组成新数组
- reduce有返回值，重点是计算数组，返回一个值

> 一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数。

## filter

> 创建一个**新**的**匹配过滤条件**的数组。
- filter() 不会对空数组进行检测
- filter() 不会改变原始数组

`array.filter(function(currentValue,index,arr), thisValue)`

返回Array 类型//符合条件的值组成的数组

- 不用filter()时

``` javascript
var arr = [
  {"name":"apple", "count": 2},
  {"name":"orange", "count": 5},
  {"name":"pear", "count": 3},
  {"name":"orange", "count": 16},
];
   
var newArr = [];
 
for(var i= 0, l = arr.length; i< l; i++){
  if(arr[i].name === "orange" ){
newArr.push(arr[i]);
}
}
 
console.log("Filter results:",newArr);
```

- 用filter()时

``` javascript
var arr = [
  {"name":"apple", "count": 2},
  {"name":"orange", "count": 5},
  {"name":"pear", "count": 3},
  {"name":"orange", "count": 16},
];
   
var newArr = arr.filter(function(item){
  return item.name === "orange";
});
 
 
console.log("Filter results:",newArr);
```

## map
> map()对数组的每个元素进行一定操作（映射）后，会返回一个新的数组。

返回 array 数组// 每个回调的返回值组成的新数组

- 不用map()时

``` javascript
var oldArr = [{first_name:"Colin",last_name:"Toh"},{first_name:"Addy",last_name:"Osmani"},{first_name:"Yehuda",last_name:"Katz"}];
 
function getNewArr(){
   
  var newArr = [];
   
  for(var i= 0, l = oldArr.length; i< l; i++){
    var item = oldArr[i];
    item.full_name = [item.first_name,item.last_name].join(" ");
    newArr[i] = item;
  }
   
  return newArr;
}
 
console.log(getNewArr());
```

- 用map()时

``` javascript
var oldArr = [{first_name:"Colin",last_name:"Toh"},{first_name:"Addy",last_name:"Osmani"},{first_name:"Yehuda",last_name:"Katz"}];
 
function getNewArr(){
     
  return oldArr.map(function(item,index){
    item.full_name = [item.first_name,item.last_name].join(" ");
    return item;
  });
   
}
 
console.log(getNewArr());
```

[你应该知道的 JavaScript Array.map() 的 5 种用途](https://juejin.im/entry/5beb69746fb9a049bd41d815?utm_source=gold_browser_extension)

## reduce
> 实现一个累加器的功能，将数组的每个值（从左到右）将其降低到一个值。

返回最后一次回调的值

相当于`[x1, x2, x3, x4].reduce(f) = f(f(f(x1, x2), x3), x4)`

### 计算重复单词
- 不用reduce()时

``` javascript
var arr = ["apple","orange","apple","orange","pear","orange"];
 
function getWordCnt(){
  var obj = {};
   
  for(var i= 0, l = arr.length; i< l; i++){
    var item = arr[i];
    obj[item] = (obj[item] +1 ) || 1;
  }
   
  return obj;
}
 
console.log(getWordCnt()); // {apple: 2, orange: 3, pear: 1}
```

- 用reduce()时

> 注意reduce内function后的{}

``` javascript
var arr = ["apple","orange","apple","orange","pear","orange"];
 
function getWordCnt(){
  return arr.reduce(function(prev,next){
    prev[next] = (prev[next] + 1) || 1;
    return prev;
  },{});
}
 
console.log(getWordCnt());
```

![](https://ws3.sinaimg.cn/large/006tNbRwgy1fujspdmnbej30dc08qjsf.jpg)


### array.reduce(fn, value)
`array.reduce(function(total, currentValue, currentIndex, arr), initialValue)`
- total	必需。初始值, 或者计算结束后的返回值。
- currentValue	必需。当前元素
- currentIndex	可选。当前元素的索引
- arr	可选。当前元素所属的数组对象。
- **initialValue 可选。传递给函数的初始值**

#### 累加

``` javascript
var numbers = [65, 44, 12, 4];
 
function getSum(total, num) {
    return total + num;
}
function myFunction(item) {
    document.getElementById("demo").innerHTML = numbers.reduce(getSum); // 125
}
```

#### 初始值的重要性
> 一般来讲prev是从数组中第一个元素开始的，next是第二个元素。但是当你**传入初始值(initialValue)后，第一个prev将是initivalValue，next将是数组中的第一个元素**。

``` javascript
var arr = ["apple","orange"];
 
function noPassValue(){
  return arr.reduce(function(prev,next){
    console.log("prev:",prev);
    console.log("next:",next);
     
    return prev + " " +next;
  });
}
function passValue(){
  return arr.reduce(function(prev,next){
    console.log("prev:",prev);
    console.log("next:",next);
     
    prev[next] = 1;
    return prev;
  },{});
}
 
console.log("No Additional parameter:",noPassValue());
console.log("----------------");
console.log("With {} as an additional parameter:",passValue());
```
![](https://ws2.sinaimg.cn/large/006tNbRwgy1fujt1qypj8j30cb05iaae.jpg)

## sort
> 数组排序
PS: 字符串根据ASCII码进行排序 eg: A-65 a-97

**Array的sort()方法默认把所有元素先转换为String再排序！**

``` javascript
['Banana', 'Apple', 'Orange'].sort(); // ['Apple', 'Banana', 'Orange'];

// 小写字母a的ASCII码在大写字母之后。
['Banana', 'apple', 'Orange'].sort(); // ['Banana', 'Orange", 'apple']

// 因为Array的sort()方法默认把所有元素先转换为String再排序，结果'10'排在了'2'的前面，因为字符'1'比字符'2'的ASCII码小。
[10, 20, 1, 2].sort(); // [1, 10, 2, 20]
```

### 添加函数

- 从小到大


方法一
``` javascript
var arr = [10, 20, 1, 2];
arr.sort(function (x, y) {
    if (x < y) {
        return -1;
    }
    if (x > y) {
        return 1;
    }
    return 0;
});
console.log(arr); // [1, 2, 10, 20]
```

方法二
``` javascript
arr.sort(function(n1, n2){
	return n1-n2;  // 可理解为：当n1-n2为正值即n1>n2时，需调换顺序，则为大的向后挪，小的向前挪 -> 升序
}); 
```
- 从大到小

方法一
``` javascript
var arr = [10, 20, 1, 2];
arr.sort(function (x, y) {
    if (x < y) {
        return 1;
    }
    if (x > y) {
        return -1;
    }
    return 0;
}); // [20, 10, 2, 1]
```

方法二
``` javascript
arr.sort(function(n1, n2){
	return n2-n1;   // 可理解为：当n2-n1为正值即n2>n1时，需调换顺序，则为大的向前挪，小的向后挪 -> 降序
}); 
```

## every
> every可以判断数组的所有元素是否满足测试条件。

``` javascript
var arr = ['Apple', 'pear', 'orange'];
console.log(arr.every(function (s) {
    return s.length > 0;
})); // true, 因为每个元素都满足s.length>0

console.log(arr.every(function (s) {
    return s.toLowerCase() === s;
})); // false, 因为不是每个元素都全部是小写
```



## forEach
> 为每个元素执行对应的方法

undefined// 这个东西没有返回值

- 不用forEach()时

``` javascript
var arr = [1,2,3,4,5,6,7,8];
 
// Uses the usual "for" loop to iterate
for(var i= 0, l = arr.length; i< l; i++){
console.log(arr[i]);
}
```

- 用forEach()时

``` javascript
arr.forEach(function(item,index){
    console.log(item);
});
```

## $.inArray()

- $.inArray(item, array)

``` javascript
handleFav: function() {
    var item = this.item;
    var favids = this.favids;
    var that = this;

    if($.inArray(item.pid, favids) > -1){
        Vue.set(that.item, 'isFav', true);
    }
}
```

## 应用

- 有一组用户信息按手机号降序排序，输出用户名称，用逗号分隔

``` javascript
[{name: 'l1', phone: '1507539'},{name: 'l2', phone: '1507540'},{name: 'l3', phone: '1507541'},{name: 'l4', phone: '1507538'}]
 .sort((n,m)=>n.phone<m.phone)
 .map(m=>m.name)
 .join(',');
 
 // "l3,l2,l1,l4"
```

- 给元素绑事件

``` javascript
[].slice.call(document.querySelectorAll('div'))
.forEach(v=>v.addEventListener('click',e=>console.log(e.target.className))
```

- 获取所有元素的class,过滤空串

``` javascript
[].slice.call(document.querySelectorAll('*'))
 .map(v=>v.className)
 .filter(v=>v)
```

- 洗牌
``` javascript
//随机排序
String.prototype.shuffle = function(arr) {
    return arr.sort(function() {
        return .5 - Math.random()
    })
}

var a = [1,3,4,0,2];
var b = a.suffle(); // [3, 1, 0, 4, 2]

```

- 赋值

``` javascript
{
  assign: (function(fruits) {
    return fruits.map(it => `<label><input type="checkbox" name="${it.name}" title="${it.title}" /> ${it.title}</label>`).join('');
  }([
    {name: 'apple', title: 'Apple'},
    {name: 'banana', title: 'Banana'},
    {name: 'orange', title: 'Orange'}
  ]))
}
```

- 数组去重

``` javascript
var r,arr = ['apple', 'strawberry', 'banana', 'pear', 'apple', 'orange', 'orange', 'strawberry'];
 
r = arr.filter(function (element, index, self) {
 return self.indexOf(element) === index;
});
 
console.log(r.toString());
```

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)