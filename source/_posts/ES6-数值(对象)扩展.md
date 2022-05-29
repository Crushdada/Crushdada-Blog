---
title: ES6-数值(对象)扩展
tags: es6
categories: Code
abbrlink: 34868
date: 2021-03-31 00:00:00
---


# ES6 数值扩展

运算打印一个浮点数，发现结果并非预期

```javascript
console.log(0.1 + 0.2); //0.30000000000000004
console.log(0.1 + 0.2 == 0.3); //false
```
<!-- more -->

为什么会有精度的问题？是因为浮点数计算不准确吗？
不，这其实与二进制存储有关

- 由于二进制存储机制，在存储十进制浮点数时，将其转化为二进制方法如下

将十进制 0.1 转化为二进制

```javascript
0.1*2 = 0.2 取0
0.2*2 = 0.4 取0
0.4*2 = 0.8 取0 
0.8*2 =1.6 取1
0.6*2 = 1.2 取1
0.2* 2 =0.4 取0
...
0.1 => 0.0001 1001 1001 1001..（无限循环）
```

- 结果为一个无限循环，但是计算机不能有限的空间存储无限的数字，因此必须舍入，这就造成了精度的问题

如此一来，在比较两个浮点数大小时，会导致 false，这不是我们想要的

#### 如何解决浮点数精度问题

- **Number.EPSILON**

ES6 提供了`Number.EPSILON`，它代表 JavaScript 最小精度，即

```javascript
2.220446049250313e-16;
```

我们只需要手写一个比较方法，判断当两个浮点数之差小于这个最小精度，就默认他俩相等

```javascript
function equal(a, b) {
  if (Math.abs(a - b) < Number.EPSILON) {
    //abs方法取绝对值
    return true;
  } else {
    return false;
  }
}
console.log(equal(0.1 + 0.2, 0.3));
```

- **Number.isFinite：**检查一个数是否为有限数
- **Number.isNAN：**检查一个数是否为 NaN
  - 该方法在 ES5 中是一个单独的函数，ES6 将其加入 Number 对象方法
- **Number.parseInt、Number.parseFloat：**字符串转 =>数字
  - 该方法会将 str 中第一个非数字及其之后的字符全部舍去

```javascript
console.log(Number.parseFloat("11.3C3ru12dada")); //11.3
```

- **Number.isInteger：**判断是否为整数
- **Math.trunc：**将小数部分抹掉
- **Math.sign：**判断一个数为正、负数、还是 0,它分别会返回三个值
  - 100=>1
  - 0=>0
  - -100=>-1
- 其他数值扩展详见[http://caibaojian.com/es6/number.html](http://caibaojian.com/es6/number.html?fileGuid=TKKD6JgYCGPJpTVq)
