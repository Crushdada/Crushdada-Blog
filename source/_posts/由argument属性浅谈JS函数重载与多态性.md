---
title: 由arguments属性浅谈JS函数重载与多态性
tags: 面经
categories: Code
abbrlink: 32282
date: 2021-05-22 00:00:00
---


# 实现函数重载

## 函数重载与面向对象多态性的关系

> 重载是一种面向对象的多态性的体现形式(或者说一种特殊的多态形式)
> JS 对于函数重载没有直接的语法支持(至少你不可以通过直接写几个同名函数来实现)
<!-- more -->

对于这一点，举个栗子：

```javascript
function add(num1, num2) {
  return num1 + num2;
}
function add(num1, num2, num3) {
  return num1 + num2 + num3;
}
console.log(add(10)); //NaN
console.log(add(10, 20)); //NaN
console.log(add(10, 20, 30)); //60
```

- 由上面例子可以看出，JS 不直接支持函数重载的，**鉴于在预编译阶段，如果发现同名函数，则用后者覆盖前者**
  > 但是我们可以通过一些方法实现

## 遍历函数的 arguments 对象属性

> > 在 ES6 之前可以借助 arguments 对象实现

### arguments 对象

它是一个**类数组**，拥有**.length 属性(可以被遍历)**,但它没有数组的一切方法，除非把它转化为真正的数组

- 可通过 Array.prototype.slice.call(arguments)从 arguments 中提取出实参数组

```javascript
let fn = function (a, b, c) {
  console.log(arguments);
  let n = Array.prototype.slice.call(arguments);
  console.log(n);
};
fn("akli", "cindela", "zhy");
//[Arguments] { '0': 'akli', '1': 'cindela', '2': 'zhy' }
//[ 'akli', 'cindela', 'zhy' ]
```

详见：[arguments 对象详解](https://www.cnblogs.com/sqh17/p/10232185.html?fileGuid=98kYCPTdGTYhHvvX)
通过遍历 arguments 对象来使 add 方法能够接收不同参数，这就实现了函数重载

```javascript
function add() {
  var sum = 0;
  for (var i = 0; i < arguments.length; i++) {
    sum += arguments[i];
  }
  return sum;
}
console.log(add(10)); //10
console.log(add(10, 20)); //30
console.log(add(10, 20, 30)); //60
```

> 那么，有一个特例--箭头函数，它是不绑定 agruments 对象的，它如何实现函数重载呢？
> ES6 引入箭头函数的同时，相信有考虑到这个问题，因此还引入了一个十分方便的新语法--**扩展运算符**，它允许我们更方便地实现函数重载 ↓

## 箭头函数--使用扩展运算符实现函数重载

```javascript
let add = (...args) => {
  let sum = 0;
  args.filter((x) => (sum += x));
  return sum;
};
console.log(add(10)); //10
console.log(add(10, 20)); //30
console.log(add(10, 20, 30)); //60
```

参考：

[JS 中的 Function 对象以及 agruments 对象详解--CSDN 博主「骑着毛驴的小猴子」](https://blog.csdn.net/weixin_31765427/article/details/77621977?fileGuid=98kYCPTdGTYhHvvX)

[浅谈 JS 函数重载](https://www.cnblogs.com/yugege/p/5539020.html?fileGuid=98kYCPTdGTYhHvvX)
