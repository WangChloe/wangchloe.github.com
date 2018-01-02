---
title: 每天10个前端知识点：数组应用篇
date: 2017-01-15 03:27:35
categories: 前端札记
tags: [js, 应用]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

{% aplayer "Wrecking Ball" "Frankmusik" "http://ojvx9eehr.bkt.clouddn.com/Frankmusik%20-%20Wrecking%20Ball.mp3" %}


---
*本次内容总结了个人遇到的部分数组应用，其中不乏前端笔试高频考点。*

---

### 1. 数组翻转方法2
eg:这里说明一下，这个方法用的不是reverse，因为一次面试中被问过不用reverse实现翻转，所以这里标注为数组的翻转方法2。
``` javascript
<script>
	var arr=[1,2,3,4];
	var arr2=[];
	while(arr.length) {
		var num=arr.pop();
		arr2.push(num);
	}
	alert(arr2);
</script>
```
<!-- more -->

### 2. 首字母大写
eg:
``` javascript
<script>
	var str = 'welcome to china';
	var arr = str.split(' ');
	var arr2 = [];
	for(var i = 0; i < arr.length; i++) {
		var first = arr[i].charAt(0).toUpperCase();
		var other = arr[i].substring(1);
		arr2.push(first + other);
	}
	alert(arr2.join(' '));

    //正则写法
	var str2 = str.replace(/\w+/g, function(s) {
		return s.charAt(0).toUpperCase().substring();
	})
	alert(str2);
</script>
```

### 3.快速清空数组
1. length=0;
2. arr=[];
3. arr.splice(0,arr.length);
4. 循环pop或shift

### 4. 数组排序方法
更多方法见后续排序算法篇

``` javascript
<script>
	function findMinIndex(arr, start) {
		var iMin = arr[start];
		var iMinIndex = start;
		for(var i = start + 1; i < arr.length; i++) {
			if(iMin > arr[i]) {
				iMin = arr[i];
				iMinIndex = i;
			}
		}
		return iMinIndex;
	}

	for(var i = 0; i < arr.length; i++) {
		var iMinIndex = findMinIndex(arr, i);
		var temp;
		temp = arr[iMinIndex];
		arr[iMinIndex] = arr[i];
		arr[i] = temp;
	}
</script>
```

### 5. 数组内查找元素是否存在
``` javascript
<script>
	function findInArr(item, arr) {
		for(var i = 0; i < arr.length; i++) {
			if(item == arr[i]) {
				return true;
			} else {
				return false;
			}
		}
	}
</script>
```

### 6. 数组去重的多种方法
#### (1)findInArr

``` javascript
<script>
	var arr2 = [];

	for(var i = 0; i < arr.length; i++) {
		if(!findInArr(arr[i], arr2)) {
			arr2.push(arr[i]);
		}
	}

	// 数组内查找元素是否存在
	function findInArr(item, arr) {
		for(var i = 0; i < arr.length; i++) {
			if(item == arr[i]) {
				return true;
			} else {
				return false;
			}
		}
	}
</script>
```

#### (2)json(自动从小到大排序)

``` javascript
<script>
	var json = {};
	var arr2 = [];

	for(var i = 0; i < arr.length; i++) {
		json[arr[i]] = 'xxx';
	}

	for(var name in json) {
		arr2.push(name);
	}
</script>
```

摘自[也谈JavaScript数组去重](http://web.jobbole.com/89843/)

``` javascript
<script>
	function unique(arr) {
        var ret = [];
        var len = arr.length;
        var tmp = {};
        for(var i=0; i<len; i++){
          if(!tmp[arr[i]]){
            tmp[arr[i]] = 1;
            ret.push(arr[i]);
        }
    }
    return ret;
}
</script>

```

#### (3)sort()

``` javascript
<script>
	arr.sort();
	for(var i = 0; i < arr.length; i++) {
		if(arr[i] == arr[i+1]) {
			arr.splice(i, 1);
			i--;
		}
	}
</script>
```

#### (4) indexOf
这个方法是在前端公众号偶然看到的，数组的indexOf方法第一次用到

``` javascript
<script>
	for(var i = 0; i < arr.length; i++) {
		if(arr2.indexOf(arr[i]) < 0) {
			arr2.push(arr[i]);
		}
	}
</script>
```

#### (5)二分法

``` javascript
<script>
	var arr = [1, 2, 3, 2, 4, 3, 1, 5, 7, 2, 5];

	// 数组内查找元素是否存在
	function findInArr(item, arr) {
		for(var i = 0; i < arr.length; i++) {
			if(item == arr[i]) {
				return true;
			}
		}
        return false;
	}

	function del(arr, s, e) {
		if(s > e) {
			return [];
		} else if(s == e) {
			return [arr[s]];
		}

		var c = Math.floor((s + e) / 2);
		var l = del(arr, s, c);
		var r = del(arr, c + 1, e);

		for(var i = 0; i < r.length; i++) {
			if(!findInArr(r[i], l)) {
				l.push(r[i]);
			}
		}

		return l;
	}

	console.log(del(arr, 0 , arr.length - 1));
</script>
```

#### (6)Map(ES6)
摘自[也谈JavaScript数组去重](http://web.jobbole.com/89843/)

Map的存取使用单独的get()、set()接口。

``` javascript
<script>

function unique(arr) {
    var ret = [];
    var len = arr.length;
    var tmp = new Map();
    for(var i=0; i<len; i++){
        if(!tmp.get(arr[i])){
            tmp.set(arr[i], 1);
            ret.push(arr[i]);
        }
    }
    return ret;
}
</script>
```

#### (7)Set(ES6)
摘自[也谈JavaScript数组去重](http://web.jobbole.com/89843/)

Set不允许重复元素出现。

``` javascript
<script>
function unique(arr){
    var set = new Set(arr);
    return Array.from(set);
}
</script>
```
---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
