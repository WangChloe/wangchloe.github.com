---
title: 每天10个前端知识点：原生篇(4)
date: 2017-01-14 02:51:22
categories: 前端札记
tags: [js]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

{% aplayer "苏荷酒吧" "James" "http://ojvx9eehr.bkt.clouddn.com/James%20-%20%E8%8B%8F%E8%8D%B7%E9%85%92%E5%90%A7.mp3" %}


---
*本次内容主要关于字符串、数组及Math的常用方法。*

---

##1. 参数的数组arguments
参数中的数组，函数中可以不需要定义参数
``` javascript
<script>
	sum(12, 5, 6);

	function sum() {
		console.log(arguments[1]);	// 5
	}
</script>

```

<!-- more -->

##2. 设置样式的三种方法
1. style.xxx
oDiv.style.width = '300px';

2. className
oDiv.className = 'active';

3. cssText
批量设置样式
oDiv.style.cssText = 'width: 300px; height: 300px';

##3. 字符串的相关方法
- str.charAt(i); 获取字符串中的第i+1个字符  返回值：相应位置的字符

  > str[i]的兼容问题
  获取字符串中的第i+1个
  - str[i]  兼容：高级浏览器及IE8+
		  IE7 -> undefined
  - str.charAt(i)  全兼容

- str.indexOf('w'); 查找w在字符串中的位置  返回值：成功 -> w在字符串中的位置  失败 -> -1

  > 
  1. 从左往右找
  2. 区分大小写
  3. 找到第一个相同值即停止
  4. 查找多个字符时，返回第一个字符的位置

-  str.lastIndexOf('w'); 查找w在字符串中的位置  返回值：成功 -> w在字符串中的位置  失败 -> -1

  > 从右往左倒序查找，返回的索引值与indexOf()规则相同

-  str.search('w'); 与indexOf()规则相同 
  > search 是强制**正则匹配**，而 indexOf 只是按字符串匹配的。

-  str.substring(开始位置, 结束位置); 截取字符串，包含开始位置，不包含结束位置

  > str.substring(开始位置); 截取字符串 **从开始位置一直截取到最后**

-  str.substr(开始位置, 截取字符串长度); 定长截取字符串

-  str.slice(开始位置, 结束位置); 截取字符串
> slice和substring的区别 
对于负数参数，slice()方法会用字符串的长度加上参数，subString()方法将其作为0处理
```
<script>
	var strObj = new String("hello world");
	alert(strObj.slice(-3));　　　　　　// 输出结果："rld"
	alert(strObj.subString(-3));　　　 // 输出结果："hello world"
	alert(strObj.slice(3,-4));　　　　 // 输出结果："lo w"
	alert(strObj.subString(3,-4))　　 // 输出结果："hel"  substring(0, 3)
</script>
```


-  str.match('w'); 在字符串中匹配w **常用于正则** 返回值：成功 -> 匹配的w  失败 -> null

-  str.split('w'); 切割字符串 **返回值类型：数组**

  > 
  1. 字符串按w割开，去掉w后组成的数组
  2. **若没找到w则原样返回一个长度为1的数组**
  3. 若为''(空字符串，无空格)则返回将str中每个字符逐个拆开的数组

-  str.toUpperCase(); str转大写
    str.toLowerCase(); str转小写

-  str.replace('xxx', 'yyy'); **常用于正则** 参数：被替换内容,替换内容

  > 
  1. **修改第一个被替换内容**
  2. **替换不修改原字符串, 需重新声明**
  3. **第二个参数可为一个方法**
  4. **replace可以连用**

``` javascript
<script>
	var str = 'xxa';
    str.replace('x','y');
    str2 = str.replace('a', 'b');
    str3 = str.replace('x', 'y').replace('a', 'b');
    alert(str);     // xxa
    alert(str.replace('x', 'y'));   // yxa
    alert(str2);    //xxb
    alert(str3);    //yxb
</script>
```
``` javascript
<script>
	var str = 'xxxy';
    var str2 = str.replace('xxx', function(s){
        alert(s);	// xxx  被替换字符 数据类型：string
        var str2 = '';
        for(var i = 0; i < s.length; i++) {
            str2 += '*';
        }
        return str2;	// 替换后的内容
    });

    alert(str2);	// ***y
</script>
```

-  str.charCodeAt(i);	// 获取字符串中的第i+1个字符对应的ASCII编码

  > a-> 0x61 -> 97
  > b-> 0x62 -> 98
  > z -> 0x7A -> 122

##4. 字符串比较
- 英文 按照字典序(a~z)依次比较，z为最大；从两字符串的第一个字符开始，若相当再比较下一个字符
- 数字 按照数字大小依次；从两字符串的第一个字符开始，若相当再比较下一个字符
- 汉字 按照unicode大小比较

##5. 字符串应用
###判断浏览器的类型
window.navigator.userAgent
eg:
``` javascript
<script>
	if(window.navigator.userAgent.indexOf('Chrome') != -1) {
		console.log('Chrome');
	} else if(window.navigator.userAgent.indexOf('Firefox') != -1) {
		console.log('Firefox');
	} else if(window.navigator.userAgent.indexOf('MSIE7.0') != -1) {
		consolle.log('IE7');
	} else {
		console.log('others');
	}
</script>
```
###判断上传文件格式
eg:
``` javascript
<script>
	var index = str.lastIndexOf('.');
   	var type = str.substring(index+1);	//返回文件类型名
</script>
```

##6. 定义数组
1. var arr = [1, 2, 3];
2. var arr = new Array(1, 2, 3);

> Array()只传一个参数时表示定义一个新数组的长度
> new Array(10); 定义一个长度为10的数组

##7. 数组的相关方法
-  arr.push('w'); 往数组最后面添加一项  返回值：新添加的那项
-  arr.unshift('w'); 往数组最前面添加一项  **返回值：新数组长度**
-  arr.pop(); 删除数组最后一项  返回值：删除的那项
-  arr.shift(); 删除数组最前一项  返回值：删除的那项
-  arr.join('w'); 数组各项用w连接成一个字符串  **返回值类型：字符串**
-  arr.concat(arr2, arr3, ...); 数组arr与arr2、arr3...连接
-  arr.reverse(); 数组翻转
-  arr.sort(); 数组排序(按字典序和数字序列)

  > 高级排序 数值排序
  - 从小到大
```
<script> 
arr.sort(function(n1, n2){
	return n1-n2;  // 可理解为：当n1-n2为正值即n1>n2时，需调换顺序，则为大的向后挪，小的向前挪 -> 升序
});
</script> 
```
  - 从大到小
```
<script> 
arr.sort(function(n1, n2){
	return n2-n1;   // 可理解为：当n2-n1为正值即n2>n1时，需调换顺序，则为大的向前挪，小的向后挪 -> 降序
});
</script> 
```
-  arr.splice(开始位置, 删除个数, 元素1, 元素2);

``` javascript
<script>
	var arr1=[1,2,3,4];
   arr1.splice(1, 0, 'a', 'b');	//添加：在1后添加'a','b'	返回值：返回空数组

   var arr2=[1,2,3,4];
   arr2.splice(1, 2); //删除：删除2、3	返回值：返回删除的各项

   var arr3=[1,2,3,4];
   arr3.splice(1, 1, 8, 88, 888) //修改：先删除再添加 把2改为8,88,888	返回值：返回删除的各项
</script>
```

> splice模拟方法
1) arr.push(c);    -> arr.splice(arr.length, 0, c);
2）arr.unshift(c); -> arr.splice(0, 0, c);
3）arr.pop();      -> arr.splice(arr.length-1, 1);
4）arr.shift();    -> arr.splice(0, 1);

##8. json(object类型)
json格式：{name:value,name2:value2, ...}
json标准格式：{"name":value, "name2":value2, ...}
> - 所有键名需双引号,键值非数字时需加引号
> - 键值对没有json.length
> - json的name是唯一的

- 获取json值: json.name 或者 json['name']
- 添加/修改: json.aaa = 'bbb'; 或者 json['aaa'] = 'bbb';
- 删除: delete json.c; 或者 delete json['c'];

- 判断json内某个属性是否存在

``` javascript
<script>
	var json = {a: 1, b: 2};
	alert('c' in json);	// false 属性c不存在
</script>
```

###json和数组的区别
####length
- 数组：有length
- json：没有length

####循环遍历方法
- 数组：for(var i=0;i<arr.length;i++){alert(arr[i])};	for循环
- json：for(var name in json){alert(json[name])};		for in循环

###访问元素下标类型
- 数组：arr[1]  数字
- json：json['a']  字符串

####顺序
- 数组：有序，根据下标访问
- json：无序，根据键名访问

##9. Math方法
1. `Math.random()`	 			0-1随机数（不包含1）
2. `Math.abs(num)`			绝对值
3. `Math.max(num1, num2, ...)`	最大数
4. `Math.min(num1, num2, ...)`	最小数
5. `Math.floor(num)`			向下取整	12.4 -> 12	12.6 -> 12
6. `Math.ceil(num)`			向上取整	12.5 -> 13	12.1 -> 13
7. `Math.pow(n, m)`			n的m次方	Math.pow(2, 3)=8;
8. `Math.sqrt(num)`			num开平方	Math.sqrt(9)=3;
9. `Math.round(num)`			四舍五入	12.1 -> 12	12.6 -> 13

> `num.toFixed(保留小数个数);`  保留几位小数(自动四舍五入)

##10. try-catch捕获异常
``` javascript
<script>
	try {
		// code
	} catch(ex) {	// exception
		console.log(ex.message);	// 查看错误信息

		// 错误的提示信息
		// 补救的代码
	}
</script>
```



---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)