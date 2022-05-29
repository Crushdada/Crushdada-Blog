---
title: Vue 特殊特性--ref
tags: Vue
categories: Code
abbrlink: 11991
date: 2021-03-22 00:00:00
---


# Vue 特殊特性

已经有几个特殊特性：key、is、slot、等已经有整理到，它们分别应用于--

- 解决 v-for 数据混乱
- 指定动态组件渲染哪个组件
- 插槽：向组件中动态插入其他元素
  > 另一个特性：**ref**
<!-- more -->

原生 JS 获取 DOM 时，要使用`document.query...`，这样会得到原生的元素，即渲染后的元素，而不是对象，如`<p>Hello World</p>`

Vue 核心是数据驱动，它不推荐这样直接操作 DOM，但有需要时，Vue 也提供了一种方法--

## ref：获取 DOM 元素

- `ref`被用来给元素或子组件注册引用信息
- 引用信息将会注册在父组件的`$refs`对象上
- 若注册了**多个**`ref`引用，同样都将会被注册在父组件的`$refs`对象上
- 如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素；
- 如果用在子组件上，引用就指向组件实例

### 为想要拿到的 DOM 命名

```xml
<p ref="name1"> Hello World </p>
<p ref="name2"> Again </p>
```

### 使用 this.$refs 获取**节点对象**

### **使用**this.$refs.ref 值 获取原生元素

注意：若用于组件，则使用`this.$refs.ref值.变量/方法名`，访问组件实例的数据或方法
