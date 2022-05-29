---
title: ES6-Symbol、迭代器、生成器
tags:
  - JavaScript
  - es6
categories: Code
abbrlink: 39178
date: 2021-03-29 00:00:00
---


# Symbol 数据类型

ES6 引入了一个内置全局函数，该函数会创建一个新的**原始**数据类型，表示独一无二的值
<!-- more -->

## 特点：

- **值\*\***唯一\*\*，可以用来解决命名冲突，当作唯一标识
- `Symbol`值不能参与运算(与自己也不行)
- `Symbol`定义的对象属性不能使用 for in 循环遍历
  - 可以使用`Reflects.ownKeys`来获取对象的所有键名

## 使用：

```javascript
let a = Symbol();
console.log(a, typeof a); // Symbol() symbol
```

创建 Symbol 的两种方式

```javascript
let s1 = Symbol("Foo");
let s2 = Symbol("Foo");
console.log(s1 == s2); //false
let s3 = Symbol.for("Bar");
let s4 = Symbol.for("Bar");
console.log(s1 == s2); //true
```

## 七种数据类型巧记

USONB -- U R SO N B

- `Undefined`
- `String、Symbol`
- `Object`
- `Null`
- `Boolean`

---

##

# 迭代器

- 迭代器`(iterator)`是一种接口，为各种不同的数据结构提供统一的访问机制。也就是说，只要数据结构种部署了该接口，那么它就可以遍历
- ES6 还引入了一个新的遍历命令与之搭配：`for...of`循环遍历

## **原生具备 iterator 接口的数据结构：**

- `Array`
- `Agruments`
- `Set`
- `Map`
- `String`
- `TypedArray`
- `NodeList`

## 底层机制

1. 数据结构中，(以数组为例)，iterator 接口表现为**一个函数**

`Array.prototype.Symbol.iterator()`

1. 它创建一个指针对象 iterator，指向数据结构的起始位置
2. 其内部有一个`next()`方法用于遍历
3. 第一次调用对象的`next`方法，指针自动指向数据结构的第一个成员
4. 不断调用`next`方法，指针一直向后移动，直到指向最后一个成员
5. 每次`next`方法返回一个包含`value`和`done`属性的对象

### 论证：

```javascript
const arr = ["浩宇", "Foo", "Bar"];
let iterator = arr[Symbol.iterator]();
console.log(arr); //找到Array的prototype中--它是一个函数
//Symbol(Symbol.iterator): function values()
console.log(iterator); //Array Iterator { }
iterator.next(); //Object { value: "浩宇", done: false }
iterator.next(); //Object { value: "Foo", done: false }
iterator.next(); //Object { value: "Bar", done: false }
iterator.next(); //Object { value: undefined, done: false }
```

## 与 for...in 区别

```javascript
const arr = ["浩宇", "Foo", "Bar"];
for (let item in arr) {
  console.log(item); //0,1,2
}
for (let item of arr) {
  console.log(item); //'浩宇','Foo','Bar'
}
```

**for ...in 保存的是键名**
**for ...of 遍历的是\*\***键值\*\*

## 迭代器应用

自定义遍历数据

举个栗子：

JS 原生**对象中并无迭代器**，此时若想随心遍历该对象中的一个数组**，**要怎样做呢--

- **手撕一个迭代器**，然后用`for ...of`遍历它

```javascript
const LPL = {
  name: "TES",
  players: ["JackeyLove", "Karsa", "knight"],
  [Symbol.iterator]() {
    let index = 0;
    return {
      next: () => {
        if (index < this.players.length) {
          const res = { value: this.players[index], done: false };
          index++;
          return res;
        } else {
          return { value: undefined, done: true };
        }
      },
    };
  },
};
for (let item of LPL) {
  //JackeyLove
  console.log(item); //Karsa
} //knight
```

---

# 生成器

ES6 提出了一个异步编程解决方案，语法行为与传统函数完全不同

- 它是一个可以**多次返回**的函数
- 用于异步编程，可解决**回调地狱**
- 它很像`Python`的生成器，这是因为“ES6 定义`generator`标准的哥们借鉴了 Python 的`generator`的概念和语法”
- 可以用`yield`返回多次

## 定义：

```javascript
function* gen() {
  //function * 函数名(){}
  console.log("Hello World");
  yield "Foo"; //在函数体中使用yield关键字，当做代码分隔符
  yield "Bar";
}
gen(); //直接调用它，并不会输出Hello World
console.log(gen()); //Generator { }
```

- 查看内部属性发现它是一个**类迭代器对象**，拥有和迭代器一样的`next`方法
- 因此，直接调用时它只是**创建**了一个**生成器对象**，而**没有执行代码段**

![图片](https://uploader.shimo.im/f/zB4s6WINpOJIx4yN.png!thumbnail?fileGuid=wJxkQrh6ChWPpGTH)

## 执行：

我们可以像使用迭代器那样使用它，因此有**两种方式**执行生成器

- 使用`next()`方法
  - `next()`方法会执行`generator`的代码，然后，每次遇到`yield`，就返回一个对象`{value: , done: true/false}`
  - 返回的`value`就是`yield`的返回值
  - `done`表示这个`generator`是否已经执行结束了
  - 如果`done`为 true，则`value`就是`return`的返回值
- 使用`for... of`

详见：[廖雪峰的官方网站：generator](https://www.liaoxuefeng.com/wiki/1022910821149312/1023024381818112?fileGuid=wJxkQrh6ChWPpGTH)

## 生成器的函数可以传参

## 生成器异步编程实例

解决回调地狱

需求：1s 后输出 111，2s 后输出 222，3s 后输出 333

传统写法

```javascript
setTimeout(() => {
  console.log(111);
  setTimeout(() => {
    console.log(222);
    setTimeout(() => {
      console.log(333);
    }, 3000);
  }, 2000);
}, 1000);
```

使用生成器

```javascript
let fn1 = () => {
  setTimeout(() => {
    console.log(111);
    iterator.next();
  }, 1000);
};
let fn2 = () => {
  setTimeout(() => {
    console.log(222);
    iterator.next();
  }, 2000);
};
let fn3 = () => {
  setTimeout(() => {
    console.log(333);
    iterator.next();
  }, 3000);
};
function* fn() {
  yield fn1();
  yield fn2();
  yield fn3();
}
let iterator = fn();
iterator.next();
```
