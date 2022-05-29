---
title: Vue-组件切换&动态组件
tags: Vue
categories: Code
abbrlink: 4227
date: 2021-02-26 00:00:00
---


1. 组件切换

对于自定义的组件，也可像普通元素一样使用 v-if 进行切换
<!-- more -->

2. 动态组件

通过 Vue 的`<component>`元素加一个特殊的`is`attribute 来实现：

```plain
<!-- 组件会在 `currentTabComponent` 改变时改变 -->
<component v-bind:is="currentTabComponent"></component>
```

在上述示例中，`currentTabComponent`可以包括

- 已注册组件的名字，或
- 一个组件的选项对象
  > **为什么要用动态组件？**

组件切换也可用 v-if、v-else，那么为什么会有**动态组件**呢--

v-if 切换原理是会将原来的元素干掉重建，因此不会保留**切换之前的状态，**如`<input>`，切换后其 value 就会消失。因此推荐使用**动态组件 ↓**

> **动态组件+keep-alive**

使用`<keep-alive>`标签将动态组件包裹，即可**缓存**切换前的(失活的)元素的状态
