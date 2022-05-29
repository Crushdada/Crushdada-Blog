---
title: Vue.config.productionTip的作用
tags: Vue
categories: Code
abbrlink: 53697
date: 2021-03-24 00:00:00
---


## Vue.config.productionTip 的作用

productionTip = true （默认）时--

![图片](https://uploader.shimo.im/f/EmbnxOVrlxOgmSle.png!thumbnail?fileGuid=TGHvCpyXcXh666D3)

productionTip = fasle 时，就没有了红框内的提示信息

<!-- more -->

**解释：**

productionTip 设置为 false ，可以阻止 vue 在启动时生成生产提示

开发环境下，Vue 会提供很多警告来帮你对付常见的错误与陷阱。而在生产环境下，这些警告语句却没有用，反而会增加应用的体积。此外，有些警告检查还有一些小的运行时开销，这在生产环境模式下是可以避免的

原文链接：[https://blog.csdn.net/yjy_191/article/details/107522014](https://blog.csdn.net/yjy_191/article/details/107522014?fileGuid=TGHvCpyXcXh666D3)
