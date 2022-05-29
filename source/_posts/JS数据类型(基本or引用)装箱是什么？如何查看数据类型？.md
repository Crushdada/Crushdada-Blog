---
title: JS数据类型(基本or引用)装箱是什么？如何查看数据类型？
tags: 面经
categories: Code
abbrlink: 32270
date: 2021-06-09 00:00:00
---


# 原始值类型

<!-- more -->

- 普通原始值类型
  - undefined
  - null
  - symbol
- 基本包装类型
  - number
  - string
  - boolean

# 引用类型

Object、Array、Function、Date、RegExp、

# 二者区别：

引用类型值可添加属性和方法，而基本类型值则不可以。

- 基本类型
  - 基本类型的变量是存放在栈内存（Stack）里的
  - 基本数据类型的值是按值访问的
  - 基本类型的值是不可变的
  - 基本类型的比较是它们的值的比较
- 引用类型
  - 引用类型的值是保存在堆内存（Heap）中的对象（Object）
  - 引用类型的值是按引用访问的
  - 引用类型的值是可变的
  - 引用类型的比较是引用的比较

# 装箱

当执行  'hello'.length  时，发现返回  5，但这并不意味着 String 是对象类型，而是因为--

JS  在执行到这条语句的时候，**内部将  'hello'  包装成了一个  String  对象**，**执行完后**，**再把这个对象丢弃了**，这种语法叫做  “**装箱**”，在其他面向对象语言里也有（如  C#）

> 因此，JS 一切皆对象的说法是错误的！原始值类型就是原始值类型，引用类型才是对象

# 查看数据类型的几种方式

## typeof

- 只能区分除去 null 外的**基本类型**`number`、`string`、`undefined`、`boolean`、其他都会判断为`object`

## Object.prototype.toString.call(sth)

- **能够精准判断是哪种数据类型，包括引用类型**
- **但是它不能判断自定义类型(自己定义的类)**

### obj.toString()方法

> > 它将   返回  "[object type]"，其中**type**是**对象的类型**,(如果此方法在自定义对象中未被覆盖)

举个栗子：

```javascript
var o = new Object();
o.toString(); // returns [object Object]
```

> > **允许我们重写对象中的该方法**

#### 重写该方法用以获取类的标识信息

- 要求**不能传入参数，并且必须返回一个字符串**。
- 自定义的`toString()`方法可以是任何我们需要的值，但如果它**附带有关对象的信息**，它将变得非常有用。
  - 比如重写构造函数对象的该方法，规定返回一个能够标识该类的信息，那么在使用该方法判断它的实例对象时，就会显示该信息

举个栗子：

```javascript
let fish = function () {};
fish.prototype.toString = function my_toString() {
  let identifier = "我是鱼类的实例对象，不是鱼雷的~_~"; //identifier:标识符
  return identifier;
};
let obj = new fish();
console.log(obj.toString()); //我是鱼类的实例对象，不是鱼雷的~_~
```

> > **或者不将标识信息写死，而设置为可以由 new 时传入它--**

```javascript
let fish = function (name) {
  this.identifier = name;
};
fish.prototype.toString = function my_toString() {
  let identifier = `我是...${this.identifier}`; //identifier:标识符
  return identifier;
};
let obj = new fish("鱼");
console.log(obj.toString()); //我是...鱼
```

当我们使用该方法来判断一个变量的类型时，由于它不一定是哪种类型，而 toString 方法是 Object 类的方法，因此不能使用该变量`.toString`的方式，需要从 Object 的原型对象上调用，然后使用 call 改变下 this 指向来判断指定的变量

#### 重写该方法用以判断自定义类型

```javascript
let fish = function () {};
fish.prototype.toString = function my_toString() {
  let identifier = this.constructor; //identifier:标识符
  return identifier;
};
let obj = new fish();
console.log(obj.toString()); //[Function: fish]
```

> 除非我们重写了 toString()方法，
> 否则我们就不能用它来判断一个自定义类型(自己定义的类)的变量
> **这时我们就需要用到 instanceof**

## instanceof

它能够直接判断出**自定义类型**

##

**参考:**

[JS 中的基本类型和引用类型](https://www.cnblogs.com/liu-di/p/11209993.html?fileGuid=GvJQjvkjKr9tv8HK)

[浅谈 Object.prototype.toString.call()方法](https://www.jianshu.com/p/585926ae62cc?fileGuid=GvJQjvkjKr9tv8HK)
