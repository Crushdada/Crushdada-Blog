---
title: Vue v-bind详解
tags: Vue
categories: Code
abbrlink: 9808
date: 2021-02-17 00:00:00
---


## v-bind

1. 使用插值表达式、`v-text`等插值语法是为**元素**绑定数据，使用`v-bind：属性名`(或 v-model)为**元素的属性**绑定数据
2. 简写为`：属性名='合法JS表达式'`如：`'temp+1'`

### 用 v-bind 为元素绑定 class 类名
<!-- more -->

应用场景：从服务器动态获取样式后动态绑定类名，实现后端控制前端样式。

注意：

1. 若使用`：class='color'`vue 会去模型的 data 中寻找这个"color"，此时--
   1. 若"color"是在<style>标签内的 css 样式类，会找不到。
   2. 若"color"是在 data 中的对象，如：

```plain
:class="['font',{'color':false}]"
```

      或`:class="obj"`

```javascript
  obj : {
    'font' : true,
    'color' : true,
    'bk-color' : false
    } //表示font、color样式类生效，bk-color不生效
      //注意类名也要用单引号括起来
```

则可控制样式是否生效

1. 若使用`：class="['color','font','bk']"`vue 会去<style>标签内寻找这几个样式类
2. 使用`v-bind`+三目运算符实现按需绑定类名--

```plain
：class="['color',flag ? 'bk1' : 'bk2']"
```

当 flag 为 true，绑定类名 bk1，否则绑定 bk2

### 用 v-bind 为元素绑定 style 样式

1. 将**样式代码(对象)**赋值给 style，但要注意 value 要加引号，样式名若含横杠"-"，也要加引号，如

`：style="{color : 'red'，'font-size': '100px'}`

2. 也可以对象的形式，将样式写在模型的 data 里，键、值都加引号：

`：style="obj"`或`：style="[obj1,obj2]"`(当绑定多个样式对象，放到数组中即可)

```javascript
obj: {
      'font-size': '100px',
      'color': 'red',
    },
```
