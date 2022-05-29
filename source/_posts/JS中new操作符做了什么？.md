---
title: JS中new操作符做了什么？
tags: 面经
categories: Code
abbrlink: 32271
date: 2021-06-10 00:00:00
---


# 1.new 操作符做了什么

new 运算符通过**调用构造函数\*\***创建一个\***\*用户定义的对象类型的\*\***实例\*\*

举个栗子：
<!-- more -->

new Array()和**[]直接赋值**，得到的变量是一样的，但 new 出来的多了一步--就是调用构造函数

> new 关键字会进行如下的操作：

- 创建一个空对象（即{}）；

_它就是我们最后要的那个实例对象，我们先要进行一些处理，让它能够继承我们指定的(传入的)构造函数，能够去访问其中的属性和方法_

- 使用 setPrototypeof 方法

设置该空对象的原型为构造函数的原型(prototype) ；

- 使用 apply 方法
  - 让构造函数的 this 指向这个空对象
  - 这样就可以通过 obj.属性名的方式，访问构造函数中的属性 ；

---

_以上三步之后，再加上一个容错机制--_

- 使用 instanceof 方法
  - 判断一下，setPrototypeof 方法是否成功，那个对象上构造函数的原型
  - 如果 new 方法失效，该函数没有返回对象，则返回刚开始创建的那个空对象。

# 2.手撕一个 new 方法

```javascript
function create(Con, ...args) {
  //Con即要new的构造函数
  // 创建一个空的对象
  let obj = Object.create(null); // 将空对象指向构造函数的原型链
  Object.setPrototypeOf(obj, Con.prototype); // obj绑定到构造函数上，便可以访问构造函数中的属性，即obj.Con(args)
  let result = Con.apply(obj, args); // 如果返回的result是一个对象则返回 // 否则new方法失效，返回obj空对象
  return result instanceof Object ? result : obj;
}
// 测试
function company(name, address) {
  this.name = name;
  this.address = address;
}
var company1 = create(company, "yideng", "beijing");
console.log("company1: ", company1);
//==>company1:  company { name: 'yideng', address: 'beijing' }
```

**参考：**
[https://blog.csdn.net/qq_39045645/article/details/105820512](https://blog.csdn.net/qq_39045645/article/details/105820512?fileGuid=vR8Jkx8PYwvg8c39)
