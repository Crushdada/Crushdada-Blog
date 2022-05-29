---
title: Vue常用指令详解
tags: Vue
categories: Code
abbrlink: 38976
date: 2021-02-17 00:00:00
---


# vue 指令

## **V-text、V-html、V-model**

1. `v-text`和`v-html`是单向绑定。
   1. `{{}}`插值表达式：将指定数据插入指定位置
   2. `v-text='xx'`，会覆盖掉元素原有内容
   3. `v-html='xx'`，也会覆盖掉原有内容，且会解析字符串中的标签元素(会转义)
2. `v-model`为双向绑定，即 view-model
<!-- more -->

## v-once

1. 该指令只初始渲染一次，不会再变化。
2. 不需要冒号表达式，如：`<p v-once></p>`

## v-cloak

1. 不需要表达式
2. 这个指令保持在元素上直到关联实例结束编译。
3. 和 CSS 规则如`[v-cloak] { display: none }`一起用时，这个指令可以隐藏未编译的 Mustache 标签直到实例准备完毕。

示例：

```css
[v-cloak] {
  display: none;
}
```

```xml
<div v-cloak>
  {{ message }}
</div>
```

## v-if、v-else-if、v-else

条件渲染，指令取值为 true 则渲染，否则**不会创建**该元素

1. 可以从模型中获取数据
2. 也可以直接赋值一个表达式，如`v-if='tmp > 10'`
3. `v-if`、`v-else`之间不能有其他元素

## v-show

1. 也用于条件渲染
2. 不同于 v-if，它会创建元素
3. 它设置了元素的`display:none`
4. 适用于经常切换元素显示-隐藏状态时，避免元素多次创建、删除消耗性能

## v-for

1. 根据数据多次渲染元素，通常用于**列表**渲染
2. v-for  指令需要使用`item in items`形式的特殊语法 其中`items`是源数据数组，而`item`则是被迭代的数组元素的别名
3. 或者通过`(value,index)`、`(value,index)`来循环**有序的**数组和**无序的**对象
4. 能够循环数字、字符串、数组、对象，如下

```xml
<ul>
  <li v-for="(value,index) in temp">{{value}}--{{index}}</li>
  <li v-for="(value,index) in str">{{value}}--{{index}}</li>
  <li v-for="(value,index) in list">{{value}}--{{index}}</li>
  <li v-for="(value,key) in obj">{{value}}--{{key}}</li>
  <li v-for="(item,index) in items">{{item.message}}--{{index}}</li>
</ul>
```

```plain
data: {
    temp: 3,
    str: 'abc',
    list: ['我','不','动'],
    obj: {
      name:'tian',
      phone:'123321',
      old:'20'  
    },
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  },
```

运行结果如下：

```plain
1--0
2--1
3--2
------------------------------
a--0
b--1
c--2
d--3
------------------------------
我--0
不--1
动--2
------------------------------
name--tian
phone--123321
old--20
------------------------------
Foo--0
Bar--1
------------------------------         
```

注意：

1. v-for="(value,key) in obj"，循环对象时，因为对象无序，所以并非是`(各项,索引)`的形式，而是`(值,名)`。vue 会将对象中各个键值对的`值`赋给第一个变量，`名`赋给第二个变量(与左右顺序有关，变量名无关)
2. v-for="(item,index) in items"，循环对象数组时，数组有序，符合`(各项,索引)`的形式 ，其中每一项 item 为一个对象，因此可用**对象.属性名**调用。
