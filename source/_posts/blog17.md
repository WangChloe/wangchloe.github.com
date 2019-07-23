---
title: 每天10个前端知识点：jQuery(上)
date: 2017-02-15 03:27:35
categories: 前端札记
tags: [jQuery]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。



---

## 1. jQuery && js

|     	 | jQuery 		     | 		js      			 |
| :--:   | 	   :----: 	     | 	 :----: 				 |
| onload |$(function() {});  |window.onload=function(){};|
|获取元素|$('div');  		 |document.getElementsByTagName('div');|
| 事件 	 |obj.click(fn); 	 | obj.onclick=fn;			 |
| this   |$(this)            |this			 			 |
| 索引   |$(this).index()    |aDiv[i].index=i;...  	     |
|操作属性|(1)获取attr(name)   |(1). 	     			 |
|        |(2)设置attr(name, value) |(2)[]	     		 |
|        |      		 	 |(3)getAttribute()/setAttribute()|

<!-- more -->

## 2. jQuery效果
- `.hide()`  隐藏
- `.show()`  显示
- `.slideDown()`  下滑
- `.slideUp()`  上滑
- `.fadeIn()`   淡入
- `.fadeOut()`  淡出

> 参数：time时间，easing运动方式，fn回调函数

- `.animate(params, [speed], [easing], fn)`

eg: `.animate({width: '100px', opacity: 1}, 'slow', {duration: 1000, easing: 'linear', complete: function(){...}})`

> animate()函数记得先清除定时器
`$('ul').stop().animate(...);`

## 3. jQuery选择器
### (1) 伪类选择器
- `$('li:first')`  第一个
- `$('li:last')`   最后一个
- `$('li:eq(n)')`  第**n+1**个
- `$('li:odd')`    第**奇数**个  (tips:记忆odd字母个数为奇数个)
- `$('li:even')`   第**偶数**个  (tips:记忆even字母个数为偶数个)
- `$('li:contains(xxx)')`  包含文本xxx的li标签
- `$('li:has(p)')`  包含p标签的li标签

### (2) 属性选择器
- `$('input[type==password]')`  属性type为password的input标签

## 4. jQuery操作css属性
- 获取 `.css('name')`;

- 设置
  - 单一样式  `.css('name', 'value')`
  - 多个样式  `.css({'name': 'value', 'name2': 'value2'})`
  eg: `.css('background-color': 'red')`或者`.css('backgroundColor': 'red')`

## 5. jQuery操作内容
- 非表单
  - 获取  `.html()`    // innnerHTML
  - 设置  `.html('xxx')`

- 表单
  - 获取  `.val()`     // value
  - 设置  `.val('xxx')`

- 文本
  - 获取  `.text()`
  - 设置  `.text('xxx')`


## 6. jQuery操作类名
- 添加  `.addClass('xxx')`
- 删除  `.removeClass('xxx')`
- 添加/删除  `.toggleClass('xxx')`

## 7. jQuery操作属性
- 获取  `.attr(name)`

- 设置
  - 单一属性  `.attr('name', 'value')`
  - 多个属性  `.attr({'name': 'value', 'name2': 'value2'})`

## 8. jQuery DOM
### (1) 创建元素
`$('<div>text</div>')`

### (2) 添加元素
- `.append()`
  parent.append(child);  父级添加子级至末尾
- `.appendTo`
  child.appendTo(parent);  子级追加至父级末尾


- `.prepend()`
  parent.prepend(child);  父级添加子级至最前
- `.prependTo()`
  child.prependTo(parent);  子级追加至父级最前


- `.before()`
  sib1.before(sib2);  sib2添加至同级sib1前面
- `.after()`
  sib1.after(sib2);   sib2添加至同级sib1后面


- `.insertBefore()`
  sib1.insertBefore(sib2);  sib1添加至同级sib2前面
- `.insertAfter()`
  sib1.insertAfter(sib2);   sib1添加至同级sib2后面


- `.wrap()`
  child.wrap(parent);  子元素被父元素包裹
- `.unwrap()`
  child.unwrap();  子元素移出上级父元素

### (3) 删除元素
- `.remove()`
  obj.remove();  删除obj节点

- `.empty()`
  obj.empty();   删除obj内所有子节点

- `.detach()`
  - $('p').detach();  删除所有p标签及其中内容
  - $('p').detach('.hello');  删除所有类为hello的p标签及其中内容

## 9. jQuery事件
**jQuery中所有事件都是绑定的**

- `.ready()`  DOM完全加载时执行的函数

- `.change()`
- `.click()`
- `.dblclick()`  双击  注意是**dbl**
- `.contextmenu()`  右击
- `.hover()`
  hover(over, out)  可以绑定两个方法

```
<script>
	obj.hover(function() {
		// 移入
	}, function() {
		// 移出
	})
</script>

```
- `.mouseover()`
- `.mouseout()`
- `.mousedown()`
- `.mouseup()`
- `.mousewheel`  鼠标滚轮  **jQuery没有封装这个函数，需用on事件绑定**
- `.keydown()`
- `.keypress()`
- `.keyup()`

> **最好不用jQuery封装的事件，都用on事件绑定**

- `.on()`  绑定
- `.off()`  解绑

- `.bind()`  绑定
- `.unbind()`  解绑

// live() die()     jQuery 1.7+被删除

[jQuery事件绑定on()、bind()与delegate() 方法详解](http://www.jb51.net/article/67166.htm)

### 事件相关
- `return false;`  阻止默认事件、阻止冒泡
- `ev.preventDefault();`  阻止默认事件
- `ev.stopPropagation();`  阻止冒泡

### 事件委托
- 第一种方法
`.on(events, selector, callback)`

```
<script>
	// $('table td').on('hover', function() {
	// 	$(this).toggleClass('active');
	// })

    // =>

	$('table').on('hover', 'td', function() {
		#(this).toggleClass('active');
	})
</script>
```

- 第二种方法
`.delegate()`

```
<script>
	$('table').delegate('td', 'hover', function() {
		$(this).toggleClass('active');
	})
</script>
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)