---
title: Vue数据绑定过程
tags: Vue
categories: Code
abbrlink: 5118
date: 2021-02-21 00:00:00
---


1. 展示未绑定数据的模板元素
2. 绑定数据并生成 HTML
3. 渲染 HTML 元素

当用户网速较慢或网站性能较差时，用户可能会看到 vue 模板内容，
<!-- more -->

因此我们需要**隐藏 vue 模板内容：**使用[v-cloak](https://cn.vuejs.org/v2/api/#v-cloak?fileGuid=9vTyVrWgJyjYtqWh)指令即可
