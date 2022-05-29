---
title: 如何判断一个数据是NaN？
tags: 面经
categories: Code
abbrlink: 32276
date: 2021-05-16 00:00:00
---


# isNaN()

不建议使用该全局方法来判断一个值是否是NaN。
<!-- more -->

* 如果`isNaN`函数的参数不是`Number`类型，`isNaN`函数会首先尝试将这个参数转换为数值，如果转换后不是数值，才返回true。
* 因此，像是能够被强制转化为数字的值来说，像是--（空字符串和布尔值分别会被强制转换为数值0和1），它们会返回`false`
* 但是，空字符串就明显“不是数值（nota number）”。
>*这种怪异行为起源于："不是数值（not a n**u**mb**e**r）"在基于IEEE-754数值的浮点计算体制中代表了一种特定的含义。*`isNaN`*函数其实等同于回答了这样一个问题：被测试的值在被强制转换成数值**时**会不会返回IEEE-754​中所谓的“不是数值（not a number）”。*
# 更好的选择--Number.isNaN()

引入自ES2015

# 或者，手撕一个isNaN()方法

一个`isNaN`的 polyfill (补丁)可以理解为（这个polyfill利用了`NaN`**自身永不相等于自身**这一特征 ）：

```javascript
var isNaN = function(value) {
    var n = Number(value);
    return n !== n;
};
```
>此外--注意new Number()和Number()的区别
```javascript
let a = new Number(123);
console.log(a instanceof Object);
//true
let a = Number(123);
console.log(a);
console.log(a === 123);
console.log(a instanceof Number);
console.log(typeof a);
//分别是123、true、false、number
```

# 栗子

```javascript
isNaN(NaN);       // true
isNaN(undefined); // true
isNaN({});        // true
isNaN(true);      // false
isNaN(null);      // false
isNaN(37);        // false
// strings
isNaN("37");      // false: 可以被转换成数值37
isNaN("37.37");   // false: 可以被转换成数值37.37
isNaN("37,5");    // true
isNaN('123ABC');  // true:  parseInt("123ABC")的结果是 123, 但是Number("123ABC")结果是 NaN
isNaN("");        // false: 空字符串被转换成0
isNaN(" ");       // false: 包含空格的字符串被转换成0
// dates
isNaN(new Date());                // false
isNaN(new Date().toString());     // true
isNaN("blabla")   // true: "blabla"不能转换成数值
                  // 转换成数值失败， 返回NaN
```
# NaN值的产生

当算术运算返回一个**未定义的或无法表示的值**时，`NaN`就产生了。

但是，`NaN`并不一定用于表示某些值**超出表示范围的情况**。

将某些不**能强制转换为数值的非数值转换为数值的时候**，也会得到`NaN`。

例如，**0 除以0**会返回`NaN`—— 但是**其他数除以0则不会返回**`NaN`。

**参考：**

[MDN | isNaN()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isNaN?fileGuid=wV9CthDjpRp9kD3X)

