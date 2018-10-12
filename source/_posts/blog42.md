---
title: nextTick研究报告
date: 2018-10-12 03:27:35
categories: 前端札记
tags: [vue]

---

- nextTick
	- es5源码
	- 关于setTimeout\(fn,0\)
- 讨论点
- 常用示例
	- 1.mounted时更新
		- console
	- 2.更改子组件更新时间
		- console
	- 3.同时更改父组件更新时间
		- console
	- 4.nextTick顺序问题
		- console
- 总结

<!-- more -->

---

## nextTick

在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

> vue这么做是因为频繁的更新dom是特别耗费性能的，所以搞了一个批处理更新，把所有的update操作放到任务队列中，等主线程中执行栈的所有同步任务执行完毕，系统就会读取任务队列。

### es5源码

``` javascript
/**
 * Defer a task to execute it asynchronously.
 * 异步更新队列
 */
var nextTick = (function() {
    var callbacks = [];
    var pending = false;
    var timerFunc;

    function nextTickHandler() {
        pending = false;

        var copies = callbacks.slice(0);
        callbacks.length = 0;
        for (var i = 0; i < copies.length; i++) {
            copies[i]();
        }
    }

    // 只要观察到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变。
    // 如果同一个 watcher 被多次触发，只会被推入到队列中一次。
    // 这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作上非常重要。
    // 然后，在下一个的事件循环“tick”中，Vue 刷新队列并执行实际 (已去重的) 工作。
    // Vue 在内部尝试对异步队列使用原生的 Promise.then 和 MessageChannel，如果执行环境不支持，会采用 setTimeout(fn, 0) 代替。
    timerFunc = function() {
        setTimeout(nextTickHandler, 0);
    };

    return function queueNextTick(cb, ctx) {
        var _resolve;
        callbacks.push(function() {
            if (cb) {
                try {
                    cb.call(ctx);
                } catch (e) {
                    handleError(e, ctx, 'nextTick');
                }
            } else if (_resolve) {
                _resolve(ctx);
            }
        });
        if (!pending) {
            pending = true;
            timerFunc();
        }
    }
})();

Vue.prototype.$nextTick = function(fn) {
    return nextTick(fn, this)
};
```

### 关于setTimeout(fn,0)

[理解 JavaScript 中的 macrotask 和 microtask](https://juejin.im/entry/58d4df3b5c497d0057eb99ff)

>  JavaScript 主线程拥有一个 `执行栈` 以及一个 `任务队列`，主线程会依次执行代码，当遇到函数时，会先将`函数 入栈`，函数运行完毕后再将该`函数 出栈`，直到所有代码执行完毕。

- macrotasks: setTimeout, setInterval, setImmediate, I/O, UI rendering
- microtasks: process.nextTick, Promises, Object.observe(废弃), MutationObserver

> 在每一次事件循环中，macrotask 只会提取一个执行，而 microtask 会一直提取，直到 microtasks 队列清空。

## 讨论点

- 父组件nextTick的触发在子组件DOM完成前还是完成后？

## 常用示例

[codepen示例](https://codepen.io/WangChloe/pen/JmEbxb)

- DOM

``` html
<div id="J_app">
	<h3>{{title}}</h3>
	<item></item>
</div>
```


### 1.mounted时更新

- 父组件

``` javascript
(function() {
    var app = new Vue({
        el: "#J_app",
        data: {
            title: "父组件"
        },

        beforeCreate: function() { // 实例初始化之后
            console.log("父组件beforeCreate");
        },
        created: function() { // 实例创建完成之后被调用
            console.log("父组件created");
        },

        beforeMount: function() { // 在挂载开始之前被调用
            console.log("父组件beforeMount");
        },

        beforeUpdate: function() { // 数据更新时调用
            console.log("父组件beforeUpdate");
        },

        updated: function() { // 数据更新之后调用
            console.log("父组件updated");
        },

        mounted: function() { // el被新创建的vm.$el替换，挂载到实例上
            var that = this;

            console.log("父组件mounted");

            this.$nextTick(function() {
                console.log("父组件nextTick");
	            console.log("当前页面content:" + that.$el.textContent);

            });

            that.title = "父组件更新";
            console.log("父组件更新");
            console.log("当前页面content:" + that.$el.textContent);
        }
    });
})();
```

- 子组件

``` javascript
(function() {
    var tpl = "<h4>{{subtitle}}</h4>";

    Vue.component('item', {
        data: function() {
            return {
                subtitle: "子组件"
            }
        },
        props: [],
        template: tpl,
        beforeCreate: function() { // 实例初始化之后
            console.log("子组件beforeCreate");
        },
        created: function() { // 实例创建完成之后被调用
            console.log("子组件created");
        },

        beforeMount: function() { // 在挂载开始之前被调用
            console.log("子组件beforeMount");
        },

        beforeUpdate: function() { // 数据更新时调用
            console.log("子组件beforeUpdate");
        },

        updated: function() { // 数据更新之后调用
            console.log("子组件updated");
        },
        mounted: function() { // el被新创建的vm.$el替换，挂载到实例上
        	var that = this;

            console.log("子组件mounted")

            this.$nextTick(function() {
                console.log("子组件nextTick");
                console.log("当前页面content:" + that.$el.textContent);
            });

            that.subtitle = "子组件更新";
            console.log("子组件更新");
            console.log("当前页面content:" + that.$el.textContent);
        }
    });
})();
```

#### console

```
父组件beforeCreate
父组件created
父组件beforeMount
子组件beforeCreate
子组件created
子组件beforeMount
子组件mounted
子组件更新
当前页面content:子组件
父组件mounted
父组件更新
当前页面content:父组件 子组件
子组件nextTick
当前页面content:子组件
父组件beforeUpdate
子组件beforeUpdate
子组件updated
父组件updated
父组件nextTick
当前页面content:父组件更新 子组件更新
```

![](https://ws4.sinaimg.cn/large/006tNbRwgy1fw0xto1eidj30wn0a80t4.jpg)

> nextTick写在数据更改前不能拿到更新后的数据。

### 2.更改子组件更新时间

``` javascript
(function() {
    var tpl = "<h4>{{subtitle}}</h4>";

    Vue.component('item', {
        data: function() {
            return {
                subtitle: "子组件"
            }
        },
        props: [],
        template: tpl,
        beforeCreate: function() { // 实例初始化之后
            console.log("子组件beforeCreate");
        },
        created: function() { // 实例创建完成之后被调用
            console.log("子组件created");
        },

        beforeMount: function() { // 在挂载开始之前被调用
            console.log("子组件beforeMount");
        },

        beforeUpdate: function() { // 数据更新时调用
            console.log("子组件beforeUpdate");
        },

        updated: function() { // 数据更新之后调用
            console.log("子组件updated");
        },
        mounted: function() { // el被新创建的vm.$el替换，挂载到实例上
            var that = this;

            console.log("子组件mounted")

            this.$nextTick(function() {
                console.log("子组件nextTick");
                console.log("当前页面content:" + that.$el.textContent);
            });

            setTimeout(function() {
                that.subtitle = "子组件更新";
                console.log("子组件更新");
                console.log("当前页面content:" + that.$el.textContent);
            }, 1000);
        }
    });
})();
```

#### console

```
父组件beforeCreate
父组件created
父组件beforeMount
子组件beforeCreate
子组件created
子组件beforeMount
子组件mounted
父组件mounted
父组件更新
当前页面content:父组件 子组件
子组件nextTick
当前页面content:子组件
父组件nextTick
当前页面content:父组件 子组件
父组件beforeUpdate
父组件updated

子组件更新
当前页面content:子组件
子组件beforeUpdate
子组件updated
```

![](https://ws3.sinaimg.cn/large/006tNbRwgy1fw0xu13un4j30wn0b6jrs.jpg)

> 若子组件在mounted即更新，子组件的update动作会在父组件的beforeUpdate之后updated之前执行。
反之则更新后子组件自己执行update，并且由于父组件nextTick写在数据更改之前，导致先执行父组件nextTick再执行父组件的update动作。

### 3.同时更改父组件更新时间

``` javascript
(function() {
    var app = new Vue({
        el: "#J_app",
        data: {
            title: "父组件"
        },

        beforeCreate: function() { // 实例初始化之后
            console.log("父组件beforeCreate");
        },
        created: function() { // 实例创建完成之后被调用
            console.log("父组件created");
        },

        beforeMount: function() { // 在挂载开始之前被调用
            console.log("父组件beforeMount");
        },

        beforeUpdate: function() { // 数据更新时调用
            console.log("父组件beforeUpdate");
        },

        updated: function() { // 数据更新之后调用
            console.log("父组件updated");
        },

        mounted: function() { // el被新创建的vm.$el替换，挂载到实例上
            var that = this;

            console.log("父组件mounted");

            this.$nextTick(function() {
                console.log("父组件nextTick");
                console.log("当前页面content:" + that.$el.textContent);

            });

            setTimeout(function() {
                that.title = "父组件更新";
                console.log("父组件更新");
                console.log("当前页面content:" + that.$el.textContent);
            }, 1000);
        }
    });
})();
```

#### console

```
父组件beforeCreate
父组件created
父组件beforeMount
子组件beforeCreate
子组件created
子组件beforeMount
子组件mounted
父组件mounted
子组件nextTick
当前页面content:子组件
父组件nextTick
当前页面content:父组件 子组件

子组件更新
当前页面content:子组件
子组件beforeUpdate
子组件updated
父组件更新
当前页面content:父组件 子组件更新
父组件beforeUpdate
父组件updated
```

![](https://ws1.sinaimg.cn/large/006tNbRwgy1fw0y07buyqj30wn0b6mxk.jpg)

> 若父、子组件的更新延时相同，则会各自执行各自的update动作，子组件优先。

### 4.nextTick顺序问题

**nextTick应写在数据更改后**

- 父组件

``` javascript
(function() {
    var app = new Vue({
        el: "#J_app",
        data: {
            title: "父组件"
        },

        beforeCreate: function() { // 实例初始化之后
            console.log("父组件beforeCreate");
        },
        created: function() { // 实例创建完成之后被调用
            console.log("父组件created");
        },

        beforeMount: function() { // 在挂载开始之前被调用
            console.log("父组件beforeMount");
        },

        beforeUpdate: function() { // 数据更新时调用
            console.log("父组件beforeUpdate");
        },

        updated: function() { // 数据更新之后调用
            console.log("父组件updated");
        },

        mounted: function() { // el被新创建的vm.$el替换，挂载到实例上
            var that = this;

            console.log("父组件mounted");

            that.title = "父组件更新";
            console.log("父组件已更新");
            console.log("当前页面content:" + that.$el.textContent);

            this.$nextTick(function() {
                console.log("父组件nextTick");
                console.log("当前页面content:" + that.$el.textContent);
            });
        }
    });
})();
```

- 子组件

``` javascript
(function() {
    var tpl = "<h4>{{subtitle}}</h4>";

    Vue.component('item', {
        data: function() {
            return {
                subtitle: "子组件"
            }
        },
        props: [],
        template: tpl,
        beforeCreate: function() { // 实例初始化之后
            console.log("子组件beforeCreate");
        },
        created: function() { // 实例创建完成之后被调用
            console.log("子组件created");
        },

        beforeMount: function() { // 在挂载开始之前被调用
            console.log("子组件beforeMount");
        },

        beforeUpdate: function() { // 数据更新时调用
            console.log("子组件beforeUpdate");
        },

        updated: function() { // 数据更新之后调用
            console.log("子组件updated");
        },
        mounted: function() {
            var that = this;

            console.log("子组件mounted")

            that.subtitle = "子组件更新";
            console.log("子组件已更新");
            console.log("当前页面content:" + that.$el.textContent);

            this.$nextTick(function() {
                console.log("子组件nextTick");
                console.log("当前页面content:" + that.$el.textContent);
            });
        }
    });
})();
```


#### console

```
父组件beforeCreate
父组件created
父组件beforeMount
子组件beforeCreate
子组件created
子组件beforeMount
子组件mounted
子组件已更新
当前页面content:子组件
父组件mounted
父组件已更新
当前页面content:父组件 子组件
父组件beforeUpdate
子组件beforeUpdate
子组件updated
父组件updated
子组件nextTick
当前页面content:子组件更新
父组件nextTick
当前页面content:父组件更新 子组件更新
```

![](https://ws4.sinaimg.cn/large/006tNbRwgy1fw0x1a7prjj30xh0b2mxj.jpg)

> 如果子组件nextTick写在数据更改前，则会在父组件mouted之后beforeUpdate之前执行nextTick。实际应在updated之后执行nextTick，这样nextTick才能拿到最新的数据。


- 推荐在updated中执行$nextTick

``` javascript
updated: function() { // 数据更新之后调用
    var that = this;

    console.log("组件updated");
    this.$nextTick(function() {
        console.log("组件nextTick");
    });
}
```

## 总结

- 父组件nextTick的触发在**子组件updated后**。

- 若子组件在mounted即更新，子组件的update动作会在父组件的beforeUpdate之后updated之前执行。**反之则更新后子组件自己执行update，不会影响父组件的nextTick**。

- 若父、子组件的更新延时相同，则会各自执行各自的update动作，**子组件优先**。

- nextTick都需写在各自**组件的updated之后**。

---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！

公众号不发文的时候推送一些我觉得好用的前端网站或者看到的一些问题的解决方案，也更便于大家交流，就酱。

![微信公众号：无媛无故](http://upload-images.jianshu.io/upload_images/2125655-f7a4736d8601eb14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)