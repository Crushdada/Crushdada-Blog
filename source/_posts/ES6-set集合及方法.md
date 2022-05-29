---
title: ES6-set集合及方法
tags: es6
categories: Code
abbrlink: 4394
date: 2021-03-28 00:00:00
---


# set 集合及方法

- 它是一种数据结构
- 类似于数组，但成员的值都是唯一的
- 实现了迭代器接口，可以使用扩展运算符和`for ...of`遍历

## 属性和方法：

- `size`：返回集合的元素个数
- `add`：增加一个新元素，返回当前集合
- `delete`：删除元素，返回一个`boolean`值
- `has`：检查集合中是否包含某元素，返回`boolean`值

<!-- more -->

注意：

- 上面说到其中值是唯一的，如果在 new 一个集合实例时，集合中有重复的值，它会**自动去重**
- `new`一个集合对象时，需要传入一个**数组，因此**常用来**将数组转化为集合**
- 使用扩展运算符和集合，灵活地对数组进行操作

## **声明一个 set**

```javascript
/*jshint esversion: 6 */
const s = new Set(["Crush", "dada", "lemon", "dada"]);
console.log(s); // Set(3) { 'Crush', 'dada', 'lemon' }
console.log(s.size); //3
```

## 集合实践

### 数组去重

```javascript
const arr = [1, 3, 5, 2, 3, 8, 4, 3];
const res = [...new Set(arr)];
console.log(res); //[ 1, 3, 5, 2, 8, 4 ]
```

### 交集

```javascript
const arr = [1, 3, 5, 2, 3, 8, 4, 3];
const arr2 = [2, 5, 3, 7];
let res = [...new Set(arr)].filter((item) => {
  let arr2ToSet = new Set(arr2);
  if (arr2ToSet.has(item)) return true;
  else {
    return false;
  }
});
//或者
//let res = [...new Set(arr)].filter(item => new Set(arr2).has(item));
console.log(res); //[ 3, 5, 2 ]
```

### 并集

```javascript
let union = [...new Set([...arr, ...arr2])];
console.log(union); //[ 1, 3, 5, 2,8, 4, 7]
```

### 差集

```javascript
let sub = [...new Set(arr)].filter((item) => !new Set(arr2).has(item)); //注意，比交集加了一个感叹号，表示取反
```

---

#

# Map

- 它是一种数据结构
- 类似于对象，是键值对的集合
  - 因此添加时，需要指定键和值两个参数
  - 但它的`key`，可以不是字符串，包括但不限于对象等
- 它也实现了迭代器接口

## 属性和方法

- `size`：返回`Map`的元素个数
- `set`：**添加**一个新元素，返回当前`map`
- `get`：根据键名拿到(返回)键值
- `has`：检查是否包含某元素，返回`boolean`
- `clear`：清空集合，返回`undefined`
- `delete`：根据根据键名删除键值对

```javascript
let map = new Map();
map.set("张三", "法外狂徒");
console.log(map); //Map(1) { '张三' => '法外狂徒' }
```

---
