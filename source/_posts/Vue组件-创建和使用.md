---
title: Vue组件-创建和使用
tags: Vue
categories: Code
abbrlink: 56453
date: 2021-02-26 00:00:00
---


# Vue 组件

> 全局组件

1. 创建组件构造器

[Vue.extend(options)](https://cn.vuejs.org/v2/api/#Vue-extend?fileGuid=qCpqddKK6yHwDk6P)

需在构造器中**编写模板**template，用于说明这个组件中要写什么元素
<!-- more -->

**注意：**

- template 中只能有一个根元素，若有多个，需放在<div>中

```javascript
let Profile = Vue.extend({
  template: "<p></p>",
});
```

在编写 template 时，由于相当于是在<script>标签内没有代码提示，代码效率低，因此，Vue 提供<template>标签专门用于编写构造器中的指定模板，使用方法如下 ↓

```xml
<template id="temp"></template>
```

然后通过**id**让`template`找到`<template>`即可

```plain
let Profile = Vue.extend({
  template: '#temp'
```

1. 注册

[Vue.component(id,[definition])](https://cn.vuejs.org/v2/api/#Vue-component?fileGuid=qCpqddKK6yHwDk6P)

**参数**：

- {string} id ：指定注册的**组件名称**
- {Function | Object} [definition]：传入创建好的构造器

```javascript
Vue.component("my-component", Profile);
```

**注意\*\***：\*\*

- 注册后的整个组件可以看作一个**自定义的标签**
- 注册时也可直接写成方法或对象，作为参数传入，更简易，但并不是省略了创建构造器的过程，而是 Vue.component 会自动调用`.extend()`,如下 ↓

```javascript
// 注册组件，传入一个扩展过的构造器
Vue.component(
  "my-component",
  Vue.extend({
    /* ... */
  })
);
// 注册组件，传入一个选项对象 (自动调用 Vue.extend)
Vue.component("my-component", {
  /* ... */
});
```

1. 使用

vue 在渲染时会将组件构造器中的**template**的内容写入自定义的组件中

```xml
<my-component></my-component>
```

> 局部组件

写入实例中，与 methods 同级（加 s 变为复数），如下 ↓

```javascript
components: {
  "组件名":{
    template: "#id"
  }
}
```

> **全局组件和局部组件的区别**

- 全局组件在任何一个 Vue 实例控制的区域都可使用
- 局部组件只能用于定义的那一个实例

4. 组件中的 data 和 methods

Vue 实例本质上也是一个组件，其中能够使用 data、methods 等，在自定义的组件中当然也可以使用。

#### data

**注意：**在**自定义组件中**不能像实例中一样直接使用 data 对象，要以**返回函数**的形式使用，在返回的函数中，**return**一个用于存放数据的**对象**

```javascript
data: function(){
  return{
    msg:"Hello World"
  }
}
```

> **为什么组件中 data 要写成\*\***函数\***\*，而不是\*\***~~对象~~\*\*

1. vue 用于复用的组件中，为了避免数据污染，不让多处的组件共享同一个 data 对象，data 以 return 的形式，将 data 看作一个函数 ，我们每次复用该组件时，都会调用一次 data 函数来生成了数据副本，返回一个新的对象。将这个新的数据对象与当前创建的组件绑定到一起。
2. 这样不会影响其他组件，因为使用 return 包裹后数据中变量只在当前组件中生效。
3. 而单纯的写成对象形式，就使得所有组件实例共用了一份 data，牵一发而动全身。
