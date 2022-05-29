---
title: Vue生命周期方法(生命周期钩子)
tags: Vue
categories: Code
abbrlink: 59859
date: 2021-03-22 00:00:00
---


## Vue 生命周期图示

![图片](https://uploader.shimo.im/f/JRkxpbPXzF6hEgbn.png!thumbnail?fileGuid=xDktdGVGQkYwqKhc)

<!-- more -->

**什么是生命周期方法**

和 Webpack 生命周期方法一样，是实例从创建到销毁整个生命过程中**各阶段调用的方法**

又名生命周期钩子、函数、事件

## 创建阶段

### beforeCreate 钩子

> 此时还没有初始化 data 和 methods，无法访问二者

### created 钩子

> 此时 data 和 methods 初始化完毕

### beforeMount 钩子

> 此时编译好 HTML 模板保存到内存，但并未渲染

### mounted 钩子

> 此时已渲染完毕

---

##

## 运行阶段

### beforeUpdate 钩子

> 当实例中的**数据变化**时调用，但此时**页面**还**没有变化**(渲染)

### updated 钩子

> 此时页面更新完毕

---

##

## 销毁阶段

组件如同实例，其内也可使用生命周期方法

### beforeDestroy 钩子

> 此时组件或实例即将销毁，但还未被销毁
> 是最后能访问 data 和 methods 的阶段

### destroyed 钩子

> 此时实例或组件已经被销毁，无法访问
