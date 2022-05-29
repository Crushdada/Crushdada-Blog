---
title: JS-Array.filter()数组过滤器
tags: JavaScript
categories: Code
abbrlink: 47861
date: 2021-03-14 00:00:00
---


## JS Array.filter()数组过滤器

    1. 该方法接收一个回调**函数**作为参数
    2. 该方法会为数组中每个元素调用一次回调函数(通过将其作为参数传入)，每次调用，要求回调函数return一个bool值
    3. 该方法会根据返回值，保留为true的元素，舍弃为false的元素
<!-- more -->

例：在一个 Array 中，**过滤掉**偶数，只保留奇数

```javascript
var arr = [1, 2, 4, 5, 6, 9, 10, 15];
var r = arr.filter(function (x) {
  return x % 2 !== 0;
});
r; // [1, 5, 9, 15]
```
