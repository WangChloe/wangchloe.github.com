---
title: vue-router使用
categories: 前端札记
tags: ['vue', 'js']
date: 2018-03-09 16:42:28
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---
<!-- MarkdownTOC -->

- 路由定义
- 子组件间通信问题
- 路由变化
- native右上角点击跳转路由

<!-- /MarkdownTOC -->


<!-- more -->

## 路由定义

- app.js

``` javascript
var router = new VueRouter({
    routes: [{
        path: '/award',
        component: {
            template: '<award></award>'
        },
        name: 'award',
    }, {
        path: '/rights',
        component: {
            template: '<rights></rights>'
        },
        name: 'rights',
    }, {
        path: '/',
        redirect: '/rights'
    }]
});

var app = new Vue({
    el: '#J_app',
    router: router,
    data: {},
    methods: {},
    mounted: function() {
        
    }
});
```

- rights.vue.js

``` javascript
var tpl = '';

Vue.component('rights', {
    // 子组件中data定义只能为return{}形式
    data: function() {
        return {}
    },
    props: [""],
    template: tpl,
    mounted: function() {

    },
    methods: {
        
    }
});
```

## 子组件间通信问题

1. 父组件中定义公共bus
`window.rightsBus = new Vue();`

2. 子组件触发并传参
``` javascript
var json = {
    'phone': that.phone,
    'award': that.award,
    'fromSource': that.fromSource
};
rightsBus.$emit("popChangeLoad", json);
```

3. 另一子组件接收参数、执行事件
``` javascript
rightsBus.$on("popChangeLoad", function(json) {
    that.award = json.award;
    that.phone = json.phone;
    that.fromSource = json.fromSource;
    that.popStep2();
}.bind(this));
```

## 路由变化

父组件中监听路由

``` javascript
var app = new Vue({
    el: '#J_app',
    router: router,
    data: {},
    methods: {},
    created: function() {
        // 实例创建后被调用
        this.routeChange();
    },
    watch: {
        // 监听$route路由变化
        '$route': 'routeChange'
    },
    methods: {
        routeChange: function() {
            // 触发子组件事件
            rightsBus.$emit("dialogClose");
        },
    },
    mounted: function() {
        
    }
});
```

## native右上角点击跳转路由

``` javascript
fl.init();

// **title图必须添加协议前缀http才能显示
fl.ready(function() {
    fl.customizeNavBar({
        "title": {
            "imgUrl": "http://static2.51fanli.net/super/src/h5/images/rights/rights/title-rights.png"
        },
        "btns": [{
            "type": 1,
            "name": "我的奖励",
            "imgUrl": "//static2.51fanli.net/common/images/loading/spacer.png",
            "text": "我的奖励",
            "cb": "rewardLoad",
            "cd": "",
            "news": 0,
        }]
    });
});

window["rewardLoad"] = function() {
    // 跳转路由
    that.$router.push('award');
}
```


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)