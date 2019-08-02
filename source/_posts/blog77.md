---
title: slot研究报告
categories: 前端札记
tags: ['vue', 'js']
date: 2019-07-26 17:07:48
---
以下内容若有问题烦请即时告知我予以修改，以免误导更多人。

---



<!-- MarkdownTOC -->

- es5源码
- es6源码
- 用法
- 单个slot
	- 显示结果
		- 进一步证实
- 具名slot
	- 显示结果
- 作用域slot
	- 显示结果
- Tips

<!-- /MarkdownTOC -->

<!-- more -->

`<slot>` 元素作为组件模板之中的内容分发插槽。`<slot>` 元素自身将被替换。

[](https://mp.weixin.qq.com/s?__biz=MzA4ODUzNTE2Nw==&mid=2451045822&idx=1&sn=0958039b4aa548dae2e5a5cdc5132a7c&chksm=87cbe4aeb0bc6db8ecf9406de4f0f517d26456c7cde9e7c83324ea09f3f3443ea1eeb96ffb38&token=1261701428&lang=zh_CN#rd)

### es5源码

``` javascript
function renderSlot (
  name,
  fallback,
  props,
  bindObject
) {
  var scopedSlotFn = this.$scopedSlots[name];
  if (scopedSlotFn) { // scoped slot
    props = props || {};
    if (bindObject) {
      extend(props, bindObject);
    }
    return scopedSlotFn(props) || fallback
  } else {
    var slotNodes = this.$slots[name];
    // warn duplicate slot usage
    if (slotNodes && "development" !== 'production') {
      slotNodes._rendered && warn(
        "Duplicate presence of slot \"" + name + "\" found in the same render tree " +
        "- this will likely cause render errors.",
        this
      );
      slotNodes._rendered = true;
    }
    return slotNodes || fallback
  }
}

function resolveSlots (
  children,
  context
) {
  var slots = {};
  if (!children) {
    return slots
  }
  var defaultSlot = [];
  for (var i = 0, l = children.length; i < l; i++) {
    var child = children[i];
    // named slots should only be respected if the vnode was rendered in the
    // same context.
    if ((child.context === context || child.functionalContext === context) &&
      child.data && child.data.slot != null
    ) {
      var name = child.data.slot;
      var slot = (slots[name] || (slots[name] = []));
      if (child.tag === 'template') {
        slot.push.apply(slot, child.children);
      } else {
        slot.push(child);
      }
    } else {
      defaultSlot.push(child);
    }
  }
  // ignore whitespace
  if (!defaultSlot.every(isWhitespace)) {
    slots.default = defaultSlot;
  }
  return slots
}

vm.$slots = resolveSlots(renderChildren, parentVnode.context);

```

## es6源码

``` javascript
export function renderSlot (
  name: string,
  fallback: ?Array<VNode>,
  props: ?Object,
  bindObject: ?Object
): ?Array<VNode> {
  const scopedSlotFn = this.$scopedSlots[name]
  let nodes
  if (scopedSlotFn) { // scoped slot
    props = props || {}
    if (bindObject) {
      if (process.env.NODE_ENV !== 'production' && !isObject(bindObject)) {
        warn(
          'slot v-bind without argument expects an Object',
          this
        )
      }
      props = extend(extend({}, bindObject), props)
    }
    nodes = scopedSlotFn(props) || fallback
  } else {
    const slotNodes = this.$slots[name]
    // warn duplicate slot usage
    if (slotNodes) {
      if (process.env.NODE_ENV !== 'production' && slotNodes._rendered) {
        warn(
          `Duplicate presence of slot "${name}" found in the same render tree ` +
          `- this will likely cause render errors.`,
          this
        )
      }
      slotNodes._rendered = true
    }
    nodes = slotNodes || fallback
  }

  const target = props && props.slot
  if (target) {
    return this.$createElement('template', { slot: target }, nodes)
  } else {
    return nodes
  }
}

export function resolveSlots (
  children: ?Array<VNode>,
  context: ?Component
): { [key: string]: Array<VNode> } {
  const slots = {}
  if (!children) {
    return slots
  }
  for (let i = 0, l = children.length; i < l; i++) {
    const child = children[i]
    const data = child.data
    // remove slot attribute if the node is resolved as a Vue slot node
    if (data && data.attrs && data.attrs.slot) {
      delete data.attrs.slot
    }
    // named slots should only be respected if the vnode was rendered in the
    // same context.
    if ((child.context === context || child.fnContext === context) &&
      data && data.slot != null
    ) {
      const name = data.slot
      const slot = (slots[name] || (slots[name] = []))
      if (child.tag === 'template') {
        slot.push.apply(slot, child.children || [])
      } else {
        slot.push(child)
      }
    } else {
      (slots.default || (slots.default = [])).push(child)
    }
  }
  // ignore slots that contains only whitespace
  for (const name in slots) {
    if (slots[name].every(isWhitespace)) {
      delete slots[name]
    }
  }
  return slots
}

function isWhitespace (node: VNode): boolean {
  return (node.isComment && !node.asyncFactory) || node.text === ' '
}

export function resolveScopedSlots (
  fns: ScopedSlotsData, // see flow/vnode
  res?: Object
): { [key: string]: Function } {
  res = res || {}
  for (let i = 0; i < fns.length; i++) {
    if (Array.isArray(fns[i])) {
      resolveScopedSlots(fns[i], res)
    } else {
      res[fns[i].key] = fns[i].fn
    }
  }
  return res
}
```

## 用法

- 当需要让组件组合使用，混合父组件的内容与子组件的模板时，就会用到slot ， 这个过程叫作内容分发（ transclusion ）。

- slot 分发的内容，作用域是在父组件上的。

- props 传递数据、events 触发事件和slot 内容分发就构成了Vue 组件的3 个API 来源，再复杂的组件也是由这3 部分构成的。

### 单个slot

- 在子组件内使用特殊的＜slot>元素就可以为这个子组件开启一个slot（插槽）。

- 在父组件模板里，插入在子组件标签内的所有内容将**替代**子组件的`<slot>` 标签及它的内容。

``` html
<div id="J_app1">
    <first-child>
        <p>父组件插入的内容</p>
    </first-child>
</div>
```

- 不使用slot

``` javascript
Vue.component('first-child', {
    template: '<div>' + 
            '<p>子组件插入的内容</p>' + 
            '</div>'
});

var app1 = new Vue({
    el: "#J_app1",
    data: {

    }
});
```

- 使用slot

``` javascript
Vue.component('first-child', {
    template: '<div>' + 
            '<p>子组件非slot内容</p>' + 
            '<slot><p>子组件插入的内容</p></slot>' +
            '</div>'
            '</div>'
});

var app1 = new Vue({
    el: "#J_app1",
    data: {

    }

});
```

#### 显示结果

- 不使用slot

``` html
<div id="J_app1">
    <div>
        <p>子组件非slot内容</p>
        <p>子组件插入的内容</p>
    </div>
</div>
```

- 使用slot

> 父组件有插入内容，所以**子组件中的slot并没有展示**。

``` html
<div id="J_app1">
    <div>
        <p>子组件非slot内容</p>
        <p>父组件插入的内容</p>
    </div>
</div>
```

##### 进一步证实

> 若父组件的插入内容为空，则默认展示子组件的额插槽内容

``` html
<div id="J_app1">
    <first-child>
        <p v-if="title">{{title}}</p>
    </first-child>
</div>
```

``` javascript
Vue.component('first-child', {
    template: '<div>' + 
            '<slot><p>子组件插入的内容</p></slot>' + 
            '</div>'
});

var app1 = new Vue({
    el: "#J_app1",
    data: {
        title: ""
    }

});
```

=> 显示结果

``` html
<div id="J_app1">
    <div>
        <p>子组件非slot内容</p>
        <p>子组件插入的内容</p>
    </div>
</div>
```

### 具名slot

- 给＜slot> 元素指定一个name 后可以分发多个内容，具名Slot 可以与单个slot 共存。

``` html
<div id="J_app2">
    <second-child>
        <h4 slot="header">Header</h4>
        <p>父组件content</p>
        <h4 slot="footer">Footer</h4>
    </second-child>
</div>
```

``` javascript
Vue.component('second-child', {
    template: '<div class="container">' +
            '<div class="header">' +
                '<slot name="header"></slot>' +
            '</div>' +
            '<div class="content">' +
                '<slot><p>子组件content</p></slot>' +
            '</div>' +
            '<div class="footer">' +
                '<slot name="footer"></slot>' +
            '</div>' +
        '</div>'
});

var app2 = new Vue({
    el: "#J_app2",
    data: {

    }
});
```

#### 显示结果

- 如果子组件的slot指定了name，将展示对应的匹配内容。

- 如果子组件的slot没有name属性，将和单个slot一样展示逻辑，父组件没有slot属性的内容都将展示在子组件的slot中。


``` html
<div class="container">
    <div class="header">
        <h4>Header</h4>
    </div>
    <div class="main">
        <p>父组件content</p>
    </div>
    <div class="footer">
        <h4>Footer</h4>
    </div>
</div>
```

### 作用域slot

- 作用域插槽是一种特殊的slot ，使用一个可以复用的模板替换己渲染元素。

- 父组件使用`<template scope="props"></template>`元素，通过scope作用域传递变量props。

``` html
<div id="J_app3">
    <third-child>
        <template scope="props">
            <p>父组件content</p>
            <p>{{props.msg}}</p>
            <p>{{props.ding}}</p>
        </template>
    </third-child>
</div>
```

``` javascript
Vue.component('third-child', {
    template: '<div class="container">' +
        '<slot msg="子组件msg" ding="子组件ding"><p>子组件content</p></slot>' +
        '</div>'
});

var app3 = new Vue({
    el: "#J_app3",
    data: {}
});
```

#### 显示结果

- slot元素上的属性可以通过`template`元素的`scope`属性传递给指定含有临时变量的插槽。


``` html
<div id="J_app3">
    <div class="container">
        <p>父组件content</p>
        <p>子组件msg</p>
        <p>子组件ding</p>
    </div>
</div>
```

## Tips

- slot不能通过`v-show`隐藏，只能通过`v-if`，因为slot是一个类似template的抽象元素，而并非一个实际元素，不能通过display控制是否显示。


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)