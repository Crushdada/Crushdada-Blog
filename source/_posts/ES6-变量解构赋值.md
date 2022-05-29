---
title: ES6-变量解构赋值变量解构赋值
tags:
  - JavaScript
  - es6
categories: Code
abbrlink: 44315
date: 2021-03-28 00:00:00
---


# ES6-变量解构赋值变量解构赋值

- ES6 允许从对象或数组中提取值，对变量进行赋值，这被称为解构赋值
- 凡是有迭代器接口的数据，就都可以解构，常见的有迭代器接口的数据就是数组、对象
<!-- more -->

## 举例：数组的解构

```javascript
const players = ["uzi", "meiko", "mlxg"];
let [a, b, c, d] = players; //声明了四个变量，对应数组中各元素的值
```

## 举例：对象的解构

```javascript
const person = {
  name: "张三",
  attribute: "法外狂徒",
  dosth() {
    console.log("搞事情");
  },
};
let { name, attribute, dosth } = person; //对象赋值解构
console.log(name); //‘张三’
dosth(); //'搞事情'，意味着可以直接使用，不必使用  对象.方法
```

## 解构赋值的本质

- 解构赋值本质是**模式匹配\*\***--\*\*
- 可以简单理解为：**等号左右两边，由数组符号[]和对象符号{}组成的架构，必须是匹配的**，元素可以多一点少一点没关系，未匹配到**默认**的为 undefined，但是模式不匹配，会直接报错。

```javascript
let [a, [[b], c]] = [1, [[2], 3]];
console.log(a); // 1
console.log(b); // 2
console.log(c); // 3
let [, , c] = [1, 2, 3];
console.log(c); // 3
let [, c] = [1, 2, 3];
console.log(c); // 2
let [a, b] = [1]; //轮不到赋值的变量，值为undefined
console.log(b);
let [a, [b, c]] = [1, 2, 3]; //模式不匹配，直接报错
a; // Uncaught TypeError: undefined is not a function
```

## 剩余符号的用法

- 剩余符号就是`...`，也就是三个点，表示剩下的我全包了

```javascript
let [a, ...b] = [1, 2, 3, 4, 5];
console.log(b); // [2, 3, 4, 5]
```

- 只允许用在最后一个变量的身前。

```javascript
let [a, ...b, c] = [1, 2, 3, 4, 5];
console.log(b);
// Uncaught SyntaxError: Rest element must be last element
```

- 如果剩余变量轮不到赋值，默认值不是 undefined，而是空数组：

```javascript
let [a, b, ...c] = [1, 2];
console.log(c); // []
```

## 为变量指定默认值

- 若该变量未匹配到，则默认值生效

```javascript
let [a, b = 3] = [1];
console.log(b); // 3
```

- 如果轮到赋值，但赋给的是 undefined，则默认值生效

```javascript
let [x, y = "3"] = ["1", undefined];
console.log(y); // 3
```

- 如果默认值是其他变量，那么要注意这个变量是否已经存在

```javascript
let [x = y, y = 1] = []; //给x赋值的时候，y还不存在，报错
// ReferenceError: y is not defined
let [x = 1, y = x] = []; //不会报错
```

- 解构对象时，若被赋值变量已经提前声明，则大括号不能放在行首，因为大括号写在行首，被 js 认为是代码块，而不是赋值的语法符号
  - 要么声明和赋值在同一行，要么用圆括弧包裹整个赋值语句

```javascript
let { y } = { y: 1 };
console.log(y); //1
let x;
({ x } = { x: 1 });
console.log(x); //1
```

- 如果**默认值是表达式**，那么只有它真的会被赋值，js 才会去计算这个表达式，如果它没有被赋值，那么 js 为了系统性能考虑，不会去计算这个表达式。

## 对象的解构赋值详解

**与数组的解构赋值不同--**

- 数组是**有序**的，因此解构时可基于**位置**(`index`)
- 对象的属性或方法**没有次序**，因此**被赋值的变量**必须与**对象属性\*\***同名\*\*，才能取到正确的值，若匹配不到同样默认为`undefined`

### 引用不匹配的属性值

如有需求，如语义化要求，声明的**变量名**与要解构对象的**属性名不一致**，如何解构？

**方法：**

- 对象解构的匹配模式，实际上是**根据**等号左边的**变量名**，去匹配对象属性的 key，找到后将其值赋给变量
- 因此，只需要写成键值对`key:val`的形式，进行模式匹配即可，这样在找到对象属性的`key`后，就会将其值赋给`val`
  - 注意，解构赋值给的变量为`val`，而`key`只是作为匹配的线索，它不是变量

```javascript
let { a: c } = { a: "aaa", b: "bbb" };
console.log(c); // "aaa"
console.log(a); // Error: a is not defined
let {
  b,
  b: { c: d },
} = { b: { c: "ccc" } };
console.log(d); // ccc
console.log(b); // {c: "ccc"}
```

## 把数组当作对象解构

JavaScript 一切皆对象，数组当然可以当做对象结构

只需要将索引`index`当作模式匹配的线索`key`即可**取出指定位置的元素**

```javascript
let { 0: x, 2: z } = ["a", "b", "c"];
console.log(x, z); // a c
```

## 字符串的解构赋值

很简单，字符串可以被拆开当做数组来解构。

```javascript
let [a, b, c, d, e] = "hello";
a; // "h"
b; // "e"
c; // "l"
d; // "l"
e; // "o"
```

由于类似数组的对象都天然有一个 length 属性，因此还可以对这个属性解构赋值。

```javascript
let { length: len } = "hello";
len; // 5
```

## 函数参数的解构赋值

之前只能这么传参，这种传参的缺陷是，你需要传 5 个参数，往往这 5 个参数来自于一个数组，于是你必须事先把这个数组拆开成 5 个变量，然后分别传进去：

```javascript
function f(a, b, c, d, e) {
  return a + b + c + d + e;
}
f(19, 19, 19, 19, 19); // 95
```

如果你强行想要一次性传入一个数组[19,19,19,19,19]，那就有点小麻烦，只能这样，这样的缺陷就是用下标表示变量，不够直观：

```javascript
function f(arr) {
  return arr[0] + arr[1] + arr[2] + arr[3] + arr[4];
}
f([19, 19, 19, 19, 19]); // 95
```

现在 ES6 之后，你就可以传入一个数组作为一套参数，js 帮你自动解构：

```javascript
function f([a, b, c, d, e]) {
  return a + b + c + d + e;
}
f([19, 19, 19, 19, 19]); // 95
```

### 默认值

函数参数也可以有默认值，都是一样的。数组元素可以空着，也可以写 undefined，但不能写 null，null 被认为是有值。

```javascript
function f([a, b, c = 100, d, e]) {
  return a + b + c + d + e;
}
f([1, 1, , 1, 1]); // 104
```

## 变量解构赋值的优点汇总

### 交换变量的值的最佳方法

```javascript
let x = 1;
let y = 2;
[x, y] = [y, x];
```

### 对数组和对象快速取值

是实践中最常用的用途，尤其是从服务器传来的 JSON，结构可能相当复杂，从前你要写多行语句来获取其中的每一个属性，现在就简单的一逼了。 比如：

```javascript
var json_from_server = [
  {
    name: "zhang",
    age: 16,
  },
  {
    name: "wang",
    age: 23,
  },
];
var [{ name: a, age: b }, { name: c, age: d }] = json_from_server;
console.log(a, b, c, d); // zhang 16 wang 23
```

如果不用结构赋值，要怎么写呢？如下：

```javascript
var json_from_server = [
  {
    name: "zhang",
    age: 16,
  },
  {
    name: "wang",
    age: 23,
  },
];
var a = json_from_server[0]["name"];
var b = json_from_server[0]["age"];
var c = json_from_server[1]["name"];
var d = json_from_server[1]["age"];
console.log(a, b, c, d); // zhang 16 wang 23
```

是不是代码很冗余？

- 就像上面说的，函数参数能解构，所以参数名既能保持语义，又能当做数组一次性传入。
- 默认值可以直接在赋值语句里定义，就不用像以往那样专门写一行语句来设定默认值，干净了很多。
- 函数参数也能设定默认值，就不用在函数体内判断参数是否存在，然后决定设不设默认值，省却很多麻烦。

## 参考文章：

- [菜鸟教程-ES6 解构赋值](https://www.runoob.com/w3cnote/deconstruction-assignment.html?fileGuid=pXTTpp6dhypx83j9)
- [简书-microkof：ES6 变量解构赋值注意事项](https://www.jianshu.com/p/8014d558a31d?fileGuid=pXTTpp6dhypx83j9)
