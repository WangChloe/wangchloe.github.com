---
title: 每天10个前端知识点：算法与数据结构
date: 2017-03-02 03:27:35
categories: 前端札记
tags: [js]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

{% aplayer "不必在乎我是谁(Live)" "林忆莲" "http://ojvx9eehr.bkt.clouddn.com/music/%E6%9E%97%E5%BF%86%E8%8E%B2%20-%20%E4%B8%8D%E5%BF%85%E5%9C%A8%E4%B9%8E%E6%88%91%E6%98%AF%E8%B0%81%28Live%29.mp3" %}

---


这篇真的写的相当好，我记得有一次见过bat的面试题是要把算法过程以柱状图形势展现。
[十大经典排序算法总结（JavaScript描述）](https://gold.xitu.io/post/57dcd394a22b9d00610c5ec8)

## 1. 查找算法
### 以有序数组查找定值为例

#### (1) 线性查找

<!-- more -->

循环遍历比较

eg: findInArr

```
<script>
	function findInArr(item, arr) {
		for (var i = 0; i < arr.length; i++) {
			if (arr[i] == item) {
				return true;
			}
		}
		return false;
	}
</script>
```

#### (2) 二分法查找

从中间开始，往左右两边查找

```
<script>
	var arr = [1, 2, 4];

	function findMid(item, start, end) {
		if (start > end) { // 起始位置不能大于结束位置
			return false;
		} else if (start == end) {
			if (arr[start] == item) {
				return true;
			} else {
				return false; // 没有找到最终走这步
			}
		}

		var mid = Math.floor((start + end) / 2); // 二分法先确定中间位置
		if (arr[mid] == item) {
			return true;
		} else {
			if (item < arr[mid]) {
				return findMid(item, start, mid);
			} else {
				return findMid(item, mid + 1, end);
			}
		}

	}

	console.log(findMid(10, 0, arr.length - 1));  // **注意此处传入的结束位置是arr.length-1
</script>
```

##### 二分法应用

掌握二分法的核心思想：**从中间开始，往左右两边查找**

- 无序数组查找最小值
	``` javascript
	<script>
		var arr = [1, 2, -4, -11, 13];

		function findMin(arr, s, e) {
			if (s > e) {
				return false;
			} else if (s == e) {
				return arr[s];
			}

			var c = Math.floor((s + e) / 2);
			var l = findMin(arr, s, c); // 先找左侧最小值
			var r = findMin(arr, c + 1, e); // 再找右侧最小值

			if (l < r) { // 两侧最小值比较
				return l;
			} else {
				return r;
			}
		}

		console.log(findMin(arr, 0, arr.length - 1));
	</script>
	```
- 二分法数组去重
	``` javascript
	<script>
		var arr = [1, 2, 3, 2, 4, 3, 1, 5, 7, 2, 5];

		// 数组内查找元素是否存在
		function findInArr(item, arr) {
			for (var i = 0; i < arr.length; i++) {
				if (item == arr[i]) {
					return true;
				}
			}
			return false;
		}

		function del(arr, s, e) {
			if (s > e) {
				return [];
			} else if (s == e) {
				return [arr[s]];
			}

			var c = Math.floor((s + e) / 2);
			var l = del(arr, s, c);
			var r = del(arr, c + 1, e);

			for (var i = 0; i < r.length; i++) {
				if (!findInArr(r[i], l)) {
					l.push(r[i]);
				}
			}

			return l;
		}

		console.log(del(arr, 0, arr.length - 1));
	</script>
	```
- 二分法数组排序(归并排序)
	``` javascript
	<script>
		var arr = [3, 1, 4, 6, 5, 7, 2, 11];

		function _sort(arr, s, e) {
			if (s > e) {
				return [];
			} else if (s == e) {
				return [arr[s]];
			}

			var c = Math.floor((s + e) / 2);
			var l = _sort(arr, s, c);
			var r = _sort(arr, c + 1, e);

			var arr2 = [];
			while (l.length > 0 || r.length > 0) {
				if (l[0] < r[0]) {
					arr2.push(l.shift());
				} else {
					arr2.push(r.shift());
				}
			}

			return arr2;
		}
	</script>
	```

## 2. 排序算法

![各种排序算法时间复杂度和空间复杂度表](http://blog.chinaunix.net/attachment/201201/18/21457204_1326898064RUxx.jpg)

### (1) 交换排序
#### 冒泡排序
每一轮循环内比较**相邻**的两个数，如果后一个比前一个小，互换位置。

- 时间复杂度：O(n^2)

``` javascript
<script>
	var arr = [3, 1, 4, 6, 5, 7, 2];

	function bubbleSort(arr) {
		var len = arr.length;
		for (var i = len; i >= 2; --i) {
			for (var j = 0; j < i - 1; j++) {
				if (arr[j + 1] < arr[j]) {
					var temp;
					temp = arr[j];
					arr[j] = arr[j + 1];
					arr[j + 1] = temp;
				}
			}
		}
		return arr;
	}

	function bubbleSort2(arr) {
		var len = arr.length;
		for (var i = 0; i <= len - 1; i++) {
			for (var j = 0; j <= len - i; j++) {
				if (arr[j + 1] < arr[j]) {
					var temp;
					temp = arr[j];
					arr[j] = arr[j + 1];
					arr[j + 1] = temp;
				}
			}
		}
		return arr;
	}

	console.log(bubbleSort(arr));
	console.log(bubbleSort2(arr));
</script>
```
#### 快速排序
采用**二分法**，取出中间数，数组**每次和中间数比较**，小的放到左边，大的放到右边。
**左边和右边再同理比较**。

- 时间复杂度：O(nlog2(n))

``` javascript
<script>
	var arr = [3, 1, 4, 6, 5, 7, 2];

	function quickSort(arr) {
		if(arr.length == 0) {
			return [];	// 返回空数组
		}

		var cIndex = Math.floor(arr.length / 2);
		var c = arr.splice(cIndex, 1);
		var l = [];
		var r = [];

		for (var i = 0; i < arr.length; i++) {
			if(arr[i] < c) {
				l.push(arr[i]);
			} else {
				r.push(arr[i]);
			}
		}

		return quickSort(l).concat(c, quickSort(r));
	}

	console.log(quickSort(arr));
</script>
```

### (2) 选择排序
#### 直接选择
每次**从当前位置**，**往后查找最小值**，与当前位置交换。

- 时间复杂度：O(n^2)

``` javascript
<script>
	var arr = [3, 1, 4, 6, 5, 7, 2];

	function selectSort(arr) {
		for (var i = 0; i < arr.length; i++) {
			var iMinIndex = findMinIndex(arr, i);
			var temp;
			temp = arr[iMinIndex];
			arr[iMinIndex] = arr[i];
			arr[i] = temp;
		}

		return arr;
	}

	function findMinIndex(arr, start) {
		var iMin = arr[start];
		var iMinIndex = start;

		for (var i = start + 1; i < arr.length; i++) {
			if(arr[i] < iMin) {
				iMin = arr[i];
				iMinIndex = i;
			}
		}

		return iMinIndex;
	}

	console.log(selectSort(arr));
</script>
```

#### 堆排序

堆排序用到**二叉树**的概念。

- 时间复杂度：O(nlog2(n))

``` javascript
<script>
	var arr = [3, 1, 4, 6, 5, 7, 2];

	function headAdjust(elements, pos, len) {
		//将当前节点值进行保存
		var swap = elements[pos];

		//定位到当前节点的左边的子节点
		var child = pos * 2 + 1;

		//递归，直至没有子节点为止
		while (child < len) {
			//如果当前节点有右边的子节点，并且右子节点较大的场合，采用右子节点和当前节点进行比较
			if (child + 1 < len && elements[child] < elements[child + 1]) {
				child ++;
			}

			//比较当前节点和最大的子节点，小于则进行值交换，交换后将当前节点定位于子节点上
			if (elements[pos] < elements[child]) {
				elements[pos] = elements[child];
				pos = child;
				child = pos * 2 + 1;
			} else {
				break;
			}

			elements[pos] = swap;
		}
	}

	//构建堆
	function buildHeap(elements) {
		//从最后一个拥有子节点的节点开始，将该节点连同其子节点进行比较，
		//将最大的数交换与该节点,交换后，再依次向前节点进行相同交换处理，
		//直至构建出大顶堆（升序为大顶，降序为小顶）
		for (var i = elements.length / 2; i >= 0; i--) {
			headAdjust(elements, i, elements.length);
		}
	}

	function sort(elements) {
		//构建堆
		buildHeap(elements);

		//从数列的尾部开始进行调整
		for (var i = elements.length - 1; i > 0; i--) {
			//堆顶永远是最大元素，故，将堆顶和尾部元素交换，将
			//最大元素保存于尾部，并且不参与后面的调整
			var swap = elements[i];
			elements[i] = elements[0];
			elements[0] = swap;

			//进行调整，将最大）元素调整至堆顶
			headAdjust(elements, 0, i);
		}
	}

	console.log(sort(arr));
</script>
```

### (3) 归并排序
采用**二分法**，左边一个**排序好的数组**，右边一个**排序好的数组**，每次**比较左右数组的第一个数**，小的放到一个新的数组中。

- 时间复杂度：O(nlog2(n))

``` javascript
<script>
	var arr = [3, 1, 4, 6, 5, 7, 2];

	function mergeSort(arr, s, e) {
		if(s > e) {
			return [];
		} else if(s == e) {
			return [arr[s]];
		}

		var c = Math.floor((s + e) / 2);
		var l = mergeSort(arr, s, c);
		var r = mergeSort(arr, c + 1, e);

		var arr2 = [];
		while(l.length > 0 || r.length > 0) {
			if(l[0] < r[0]) {
				arr2.push(l.shift());
			} else {
				arr2.push(r.shift());
			}
		}

		return arr2;
	}

	console.log(mergeSort(arr, 0, arr.length - 1));
</script>
```

## 3. 数据结构

- 时间复杂度
- 空间复杂度

![各种排序算法时间复杂度和空间复杂度表](http://blog.chinaunix.net/attachment/201201/18/21457204_1326898064RUxx.jpg)

### (1) 有序数组
### (2) 无序数组
不重复

``` javascript
<script>
	var unorder_arr = [];

	function unorder_find(n) {
		for (var i = 0; i < unorder_arr.length; i++) {
			if(unorder_arr[i] == n) {
				return true;
			}
		}

		return false;
	}

	function unorder_add(n) {
		if(!unorder_find(n)) {
			unorder_arr.push(n);
		}
	}

	unorder_add(33);
	unorder_add(16);
	unorder_add(41);
	unorder_add(22);

	console.log(unorder_arr);
</script>
```


### (3) 二叉树
增加、查找

以第一个树为根节点，新进来的数比谁小跟谁近就放在谁下面

```
根
	节点: {
		value: n,
		l: null,
		r: null
	}

```

``` javascript
<script>
	var root = null;

	function add(node, n) {
		if (root == null) {
			root = {
				value: n,
				l: null,
				r: null
			};
		} else {
			if (n == node.value) {
				console.log('不能重复');
				return;
			} else {
				if (n < node.value) {
					console.log('查看左子树');
					if (node.l == null) {
						node.l = {
							value: n,
							l: null,
							r: null
						}
					} else {
						console.log('左子树不为空');
						return add(node.l, n);
					}
				} else {
					console.log('查看右子树');
					if (node.r == null) {
						node.r = {
							value: n,
							l: null,
							r: null
						}
					} else {
						console.log('右子树不为空');
						return add(node.r, n);
					}
				}
			}
		}
	}

	add(root, 42);
	add(root, 33);
	add(root, 66);
	add(root, 88);
	add(root, 1);
	add(root, 50);
	console.log(root);
</script>
```

### (4) 队列
特点：先进先出，后进后出

### (5) 堆栈
特点：后进先出，先进后出

### (6) 散列

存的时候先开辟一块空间出来

``` javascript
<script>
	var hash_arr = [];
	hash_arr.length = 5;
	var count = 0;

	function hash_add(n) {
		var pos = n % hash_arr.length;
		if (hash_arr[pos]) {
			while (hash_arr[pos]) {
				if (hash_arr[pos] == n) return;
				pos++;
				if (pos == hash_arr.length - 1) {
					pos = 0;
				}
			}
			hash_arr[pos] = n;
		} else {
			hash_arr[pos] = n;
		}

		count++;
		if (count == hash_arr.length) {
			var oldArr = hash_arr;
			hash_arr = [];
			count = 0;
			hash_arr.length = oldArr.length * 2;
			for (var i = 0; i < oldArr.length; i++) {
				hash_add(oldArr[i]);
			}
		}
	}

	hash_add(35);
	alert(hash_arr);
	hash_add(42);
	alert(hash_arr);
	hash_add(9);
	alert(hash_arr);
	hash_add(22);
	alert(hash_arr);
	hash_add(11);
	alert(hash_arr);
	hash_add(46);
	alert(hash_arr);
	hash_add(32);
	alert(hash_arr);
	hash_add(7);
	alert(hash_arr);
	hash_add(2);
	alert(hash_arr);
	hash_add(12);
	alert(hash_arr);
	hash_add(31);
	alert(hash_arr);
</script>
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)