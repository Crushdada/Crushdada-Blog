---
title: Vue v-on详解
tags: Vue
categories: Code
abbrlink: 8563
date: 2021-02-18 00:00:00
---


## v-on

1. 为元素绑定监听事件
2. `v-on:事件名="函数名"`，简写`@事件名='函数名'`
3. `v-on`绑定的事件触发后，vue 会去实例对象的`methods`中找对应的回调函数
4. 使用修饰符，如`v-on:事件名.once="函数名"`
5. 使用`@事件名='函数名($event)'`来获取事件对象
<!-- more -->

### .once

默认只要事件触发，回调函数会反复执行，若只想让它执行一次，可使用 once 修饰符：

### .prevent

阻止监听的元素本身的默认行为(而非监听事件的)

### .stop

**嵌套**的元素中，多个元素监听到发生同一事件时，会发生事件冒泡，可用 stop 修饰符阻止。

### .self

让回调函数只有当前元素触发事件时才执行(即避免**被其他元素**冒泡**影响**，但该元素触发监听事件**可影响其他元素**)

### .capture

添加事件侦听器时使用 capture 模式，将事件冒泡变成事件捕获

### v-on 按键修饰符

用来监听按键，可参考[Vue.js 文档](https://cn.vuejs.org/v2/guide/events.html#%E6%8C%89%E9%94%AE%E4%BF%AE%E9%A5%B0%E7%AC%A6?fileGuid=KwXpw88YWVGPHDJv)--

1. 文中建议使用内置的按键别名，但在不满足需求的情况下，可通过 Keycode 自定义按键名
2. Keycode：键盘上每个键都对应一个数字，这个数字就是键的 Keycode
3. 知道了键的 Keycode 就可以在 JS 代码位置根据 Keycode 自定义键的别名：

```javascript
Vue.config.keyCodes.f2 = 113;
```
