---
title: 每天10个前端知识点：原生篇(1)
date: 2017-01-11 03:27:35
categories: 前端札记
tags: [js]

---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

{% aplayer "I Really Like You" "Anthem Lights" "http://ojvx9eehr.bkt.clouddn.com/Anthem%20Lights%20-%20I%20Really%20Like%20You.mp3" %}


---

*本次内容主要关于js数据类型及变量命名。*

---
## 1. js六大数据类型

> null并非typeof出来的类型，不过由于null不可再分，所以将其归于基本数据类型之中。

> typeof类型：undefined、boolean、string、number、object、function

**以下是根据ECMAScript标准的数据类型分类**

<!-- more -->

### 基本数据类型
- Number    数字
- String    字符串
- Boolean   布尔
- Undefined 未定义
- **Null 空对象**

- Symbol  独一无二的值(ES6新增)

### 复杂数据类型
- Object    对象(可拆分为多种数据类型)

## 2. 数据类型补充

- null空对象 -> 数据类型(object)
- NaN 非数字 -> 数据类型(number)

>  NaN和任何数据类型都不相等，包括自己

## 3. 数字相关判断方法

- ### 是否是数字
isNaN() 非数字->true  数字->false
- ### 是否是整数
if(num == parseInt(num))

## 4. 变量

1. ### 全局变量
2. ### 局部变量
3. ### 闭包
	  子函数可以使用父函数的全局变量

> 变量的遮蔽
  全局变量和局部变量同名
  就近原则->在函数里优先使用自己的变量

## 5. 运算符

1. ### 算术运算符
2. ### 比较运算符
		== -> !=	=== -> !==
3. ### 赋值运算符
4. ### 逻辑运算符

## 6. 常见变量命名前缀
| 前缀 | 		全称      |     含义 	 		 | 示例       |
| :--: | 	   :----: 	  |   :----: 	 		 | :---:      |
| o    | 	   object  	  |   一个对象，一个元素 | oDiv       |
| a    | 	   array 	  |   一组元素 			 | aLi        |
| s    | 	   string 	  |   字符串 			 | sUserName  |
| i    | 	   integer    |   整数 				 | iCount     |
| f    | 	   float 	  |   浮点数 			 | fPrice	  |
| b    | 	   boolean    |   布尔 				 | bOk		  |
| fn   | 	   function   |   函数 				 | fnSucc	  |
| re   | 	   RegExp     |   正则 				 | reMailCheck|

## 7. 字符串转化为数字
### parseInt()
- 从左往右开始找，找到第一个非数字(包含小数点)就停止，如果第一个数不是数字，则返回NaN
- eg: '12.5' -> 12	'12abc' -> 12	'abc' -> NaN

### parseFloat()
- 从左往右开始找，找到第一个非数字(不包含小数点)就停止，如果第一个数不是数字，则返回NaN
- eg: '12.5' -> 12.5	'12abc' -> 12	'abc' -> NaN

### Number()
- 既能处理整数，也能处理小数，但只能处理数字
- eg: '12.5' -> 12.5 	'12' -> 12 		'12abc' -> NaN 	'abc' -> NaN

## 8. 数字转化为字符串
number + ''

> eg：12 + '' -> '12'

## 9. if语句变形
1. 条件 && 语句; (条件为真时执行)
1. 条件 || 语句; (条件为假时执行)
1. 三目运算  条件? 语句1: 语句2;

## 10. **js**及**事件**的笼统概念
- js：修改样式
- 事件：用户的操作

>任何标签都可以添加事件，任何属性都可以修改


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)