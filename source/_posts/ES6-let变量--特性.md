---
title: ES6-let变量--特性
tags:
  - JavaScript
  - es6
categories: Code
abbrlink: 57965
date: 2021-03-26 00:00:00
---


# let 变量--特性

## let 不能重复声明

防止变量污染

```javascript
let a = "Crushdada";
let b = "Crushdada";
```
<!-- more -->

## 块级作用域

ES5 有三种作用域：全局、函数、eval(严格模式)

ES6 引入了新作用域--

**块级作用域：**只在**代码块**中有效，在外部读取不到

```javascript
//if else while for等的花括弧中若使用let，也为块级作用域
{
  var a = "crush"; //全局作用域
  var b = "dada"; //块级作用域，只在该花括弧中有效
}
```

## 不会变量提升

### **变量提升：**

JS 允许先使用，后声明变量，原因是它会在编译之前，从程序中找出允许变量提升的变量，将其放到代码段最开始，因此相当于还是先声明再使用。

**注意：**“声明”和“初始化”是不同的

```javascript
var a; //声明变量
var b = 1; //初始化变量
```

应当将`变量提升`更贴切地称为--声明提升，它只会将声明提升到顶部，而不会将初始化的值提升，因此 ↓

```javascript
var a = 5;
console.log(a, b); //5，undefined
var b = 10;
//let不会变量提升
```

而 let--

```javascript
console.log(c);
let c； //Error：Cannot access 'c' before initialization
```

## 不影响作用域链

要理解什么是作用域链，我们先要了解**JS 作用域**

#### JS 作用域:

- **全局作用域(Global Scope)**
  - 最外层函数和在最外层函数外面定义的变量拥有全局作用域
  - 所有末声明直接赋值的变量自动声明为拥有全局作用域，like`a = 10`
  - 所有 window 对象的属性拥有全局作用域。
- **局部作用域(Local Scope)**, 只有局部代码才能访问到的作用域。例如函数。

#### 作用域链:

- 函数对象内部有一个只能由 JS 引擎访问到的属性[[scope]],它指向的就是作用域链，这个作用域链中包含了此函数内部所能访问到的所有数据。
- 其中链的**头部是当前函数的活动对象**(AO)，如果存在多个函数嵌套，接下来指向的是当前函数上层函数的活动对象(AO), 依次类推；
- 无论存没存在函数嵌套，作用域链**尾部都是全局对象**。
- 当访问一个数据时，会沿着作用域头部一直往后找，直到找到全局对象，如果都没有，将报错。

参考文章：[JavaScript 内部属性[[Scope]]与作用域链及其性能问题](https://blog.csdn.net/q1056843325/article/details/53086893?fileGuid=cwpXGrDTjVVDrXV6)

#### 变量对象与活动对象：

- 变量对象：
  - 编译时所创建，不可被访问.
  - 变量对象包括函数声明时的参数，内部声明的变量以及函数。
- 活动对象：
  - 还是变量对象本身，只不过是因为处在执行阶段可以被访问所以成了活动对象
- 二者区别：处于程序运行时的不同周期，为同一个对象。（或者是处于执行上下文的不同生命周期）

参考文章：

- [JS 中作用域与作用域链总结](https://blog.csdn.net/zhang1339435196/article/details/102557086?fileGuid=cwpXGrDTjVVDrXV6)
- [尚硅谷-let 经典案例实践](https://www.bilibili.com/video/BV1uK411H7on?p=4&spm_id_from=pageDriver&fileGuid=cwpXGrDTjVVDrXV6)
