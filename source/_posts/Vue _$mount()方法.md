---
title: Vue $mount()方法
tags: Vue
categories: Code
abbrlink: 64308
date: 2021-03-19 00:00:00
---


## Vue.$mount()方法

用来将自己手撕的[全局 API 扩展](https://cn.vuejs.org/v2/api/#Vue-extend?fileGuid=PGDjyQVV8yrJgJ63)或实例**手动挂载**到 DOM 上

- 项目中可用于延时挂载(如在挂载前先进行其他操作、判断等)，之后再手动挂载上
- new Vue 时，$mount 和**el**并没有本质上的不同
<!-- more -->

方法：

```javascript
const app = new Vue({
  //实例
  //...
}).$mount("#app");
var judy = Vue.extend({
  //扩展
  //...
});
var vm = new judy().$mount("#app");
```

```xml
<div id="app">
  {{message}}
</div>
```
