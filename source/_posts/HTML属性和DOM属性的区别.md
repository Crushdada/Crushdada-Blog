---
title: HTML属性和DOM属性的区别
tags: HTML
categories: Code
abbrlink: 36925
date: 2021-03-15 00:00:00
---


# HTML 属性和 DOM 属性的区别

## 页面是如何形成的--
<!-- more -->

1. 请求 HTML 代码：浏览器发出请求，TCP/IP 三次握手后，服务器返回给 HTML 代码
2. 解析 HTML：浏览器使用 HTML 解析器解析 HTML 代码并构建 DOM 树
3. 解析 CSS：浏览器使用 CSS 解析器构建样式表规则
4. DOM 嵌套：浏览器按从上到下，从左到右的顺序将 DOM 树压入文档流，检测节点间的层级结构，完成嵌套
5. DOM 布局：将嵌套完成的 DOM 节点布局到页面上
6. 渲染 DOM

综上，DOM 元素是经浏览器解析后的 HTML 元素

- HTML 元素属性--attribute
- DOM 元素属性--property

更多资料参考：[https://blog.csdn.net/weixin_44296929/article/details/103457150](https://blog.csdn.net/weixin_44296929/article/details/103457150?fileGuid=9wQVGCgHvVj9Vx9C)
