---
title: ES6-对象方法的扩展
tags: es6
categories: Code
abbrlink: 49931
date: 2021-04-01 00:00:00
---


# ES6 对象方法的扩展

## Object.is()：

- 判断两个值是否相等
<!-- more -->

```javascript
Object.is(10, 10); //true
Object.is(NaN, NaN); //true
```

## Object.assign()：

- 对象的合并，常用作合并用于存储配置的对象
  - 两个参数 | (obj，obj)
  - **后者会覆盖前者的\*\***重名\***\*属性和方法**

## Object.setPrototypeOf()：

- 为一个对象设置它的原型对象

```javascript
const person = {
  dosth: "play lol",
};
const animal = {
  attr: ["eat", "drink"],
};
Object.setPrototypeOf(person, animal);
console.log(person);
```

对于 person 对象来说，它的原型链：`person=>object`
该方法向原型链中插入了一个原型对象 animal

它现在的原型链：`person=>animal=>object`

![图片](https://uploader.shimo.im/f/gB1uqpdHl5tmREeW.png!thumbnail?fileGuid=VTPdGJpyTvHpVdKR)

---

##

> **ES8 对象方法扩展**

## Object.keys()：

- 获取对象所有键，返回一个数组

```javascript
let obj = {
  a: "1",
  cities: ["a", "b", "c"],
};
console.log(Object.keys(obj)); //[ 'a', 'cities' ]
```

## Object.values()：

- 获取对象所有值，返回一个数组(数组内的元素还是键值原来的类型)

## Object.entries()：

- 将对象中所有键值对梳理成条目，返回一个二维数组，或者可以理解为--
- 返回一个数组，该数组的每个元素都是一个数组，用以存放键和值
- 对象中有多少对键值对，该数组就有多少个类型为数组的元素

```javascript
let obj = {
  name: "examp",
  skills: ["a", "b", "c"],
  fav_foods: ["Foo", "Bar", "aoligei"],
};
console.log(Object.entries(obj));
//结果为
[
  ["name", "examp"],
  ["skills", ["a", "b", "c"]],
  ["fav_foods", ["Foo", "Bar", "aoligei"]],
];
//该方法为创建Map提供便利
const res = new Map(Object.entries(obj));
console.log(res.get("skills")); //[ 'a', 'b', 'c' ]
```

## Object.getOwnPropertyDescriptors()：

- 获取对象属性的**描述对象，**返回**一个对象**，在该对象中可以看到各属性的相关描述

```javascript
let obj = {
  name: "examp",
  skills: ["a", "b", "c"],
  fav_foods: ["Foo", "Bar", "aoligei"],
};
const res = Object.getOwnPropertyDescriptors(obj);
console.log(res);
```

结果为：
![图片](https://uploader.shimo.im/f/ZjHl2ePeeqhOOurV.png!thumbnail?fileGuid=VTPdGJpyTvHpVdKR)

- 它既然可以拿到对象属性的描述，那么--它可以被应用于深度克隆(Deep clone)
  - 根据指定的原型对象的描述，来配置 clone 的新对象的描述

```javascript
const obj_copy = Object.create(null, {
  name: {
    value: "examp",
    writable: true,
    enumerable: true,
    configurable: true,
  },
});
console.log(obj_copy.name);
console.log(obj.name);
```

- Object.create 是 ES5 方法
  - 用于创建一个新对象
  - 同时能够指定新对象的原型(_proto_)以及**描述**
  - 参数：| (原型对象，对象属性及描述)
