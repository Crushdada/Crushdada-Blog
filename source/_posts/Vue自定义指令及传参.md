---
title: Vue自定义指令及传参
tags: Vue
categories: Code
abbrlink: 20133
date: 2021-02-19 00:00:00
---


## 自定义全局指令

1. 使用`Vue.directives()`方法，两个参数：字符串(指令名)、对象(业务逻辑)
2. 注意：自定义指令名称不要加 v-，vue 会自动加上，用的时候如：<p v-color></p>
3. 在对象中(实现业务逻辑时)，使用[钩子函数](https://cn.vuejs.org/v2/guide/custom-directive.html#%E9%92%A9%E5%AD%90%E5%87%BD%E6%95%B0?fileGuid=6gqKHvxhTHtcPDGW)规定 Vue 何时执行你的函数--
<!-- more -->

```xml
 <p v-color>Hello World</p>
```

```javascript
Vue.directive("color", {
  bind: function (el) {
    el.style.color = "red";
  },
});
```

## 为自定义指令传参

指令钩子函数会被传入 四个参数：

- `el`：指令所绑定的元素，可以用来直接操作 DOM。
- `binding`：一个对象，包含许多属性
- `vnode`：Vue 编译生成的虚拟节点。移步[VNode API](https://cn.vuejs.org/v2/api/#VNode-%E6%8E%A5%E5%8F%A3?fileGuid=6gqKHvxhTHtcPDGW)来了解更多详情。
- `oldVnode`：上一个虚拟节点，仅在`update`和`componentUpdated`钩子中可用。

允许在业务逻辑中获取它们 ，详见[钩子函数参数](https://cn.vuejs.org/v2/guide/custom-directive.html#%E9%92%A9%E5%AD%90%E5%87%BD%E6%95%B0%E5%8F%82%E6%95%B0?fileGuid=6gqKHvxhTHtcPDGW)

可直接在元素中赋值传参，也可将值放在 model 的 data 中，如下 ↓

```xml
<p v-color="'blue'">Hello World</p>
<p v-color="curColor">Hello World</p>
```

```javascript
Vue.directive("color", {
  bind: function (el, obj) {
    el.style.color = obj.value;
  },
});
```

```javascript
data: {
  curColor: "red";
}
```

## 自定义局部指令

局部：作用域为**实例内**，不可用于其他 vue 实例

定义在实例中与 data 同级的 directives 对象内

```javascript
let vue1 = new Vue({
  el: "#app1",
  data: {},
  methods: {},
  directives: {
    指令名: {
      业务逻辑,
    },
  },
});
```
