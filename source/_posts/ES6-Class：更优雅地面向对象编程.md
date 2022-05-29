---
title: ES6-Class：更优雅地面向对象编程
tags: es6
categories: Code
abbrlink: 25513
date: 2021-03-30 00:00:00
---


# Class 类

- ES6 引入了‘类’这一概念，作为对象模板，更接近传统语言的写法(面向对象)
- 但它只是 ES5 的语法糖，它的绝大多数功能 ES5 都可实现
- 引入它是为了更像“面向对象”编程的语法

<!-- more -->

## 对比 ES5 的面向对象编程--

- ES5 使用构造函数来实例化对象

```javascript
//声明类
function phone(brand, price) {
  this.brand = brand;
  this.price = price;
}
//添加方法
phone.prototype.call = function () {
  console.log("Make a phone call");
};
//实例化对象
const Huawei = new phone("华为", "3000");
Huawei.call(); //Make a phone call
console.log(Huawei); //phone { brand: '华为', price: '3000' }
```

## ES6：使用 class 类实现面向对象

- **声明一个类**
- **写它的构造方法**：class 类中必须有一个名为`constructor`的构造方法
  - 当使用`new`实例化时，会自动调用它
- **添加方法**：在 class 内部使用必须使用`函数名(){}`的格式，不能使用 ES5 的**对象完整形式**

```javascript
class phone {
  constructor(brand, price) {
    this.brand = brand;
    this.price = price;
  }
  call() {
    console.log("Make a phone call");
  }
}
```

- 实例化与 ES5 相同

## class 静态成员

```javascript
function phone() {
  //这是一个构造函数
}
phone.name = "phone"; //构造函数也是一个对象,为它添加属性和方法
phone.change = () => {
  console.log("I can change the world!");
};
//实例化对象
const Huawei = new phone();
console.log(Huawei.name); //undefined
```

- 上面用**实例对象**访问**构造函数对象**的属性，结果为`undefined`，说明它们是**不同**的

那么，实例对象和谁相同呢？

- 实例对象和**构造函数的\*\***原型对象\*\*的属性和方法是相同的

```javascript
function phone() {
  //这是一个构造函数
}
phone.prototype.name = "telephone";
const Huawei = new phone();
console.log(Huawei.name); //telephone
```

- 可以在类中声明`static`静态成员，只属于类，不属于实例对象(无法访问)

## 构造函数继承

ES5 的构造函数继承

```javascript
function Phone(brand, price) {
  this.brand = brand;
  this.price = price;
}
//为构造函数对象添加方法
Phone.prototype.call = function () {
  console.log("I can call u!");
};
function smartphone(brand, price, color, size) {
  Phone.call(this, brand, price);
  this.color = color;
}
//设置级构造函数的原型,这样来继承父级的方法
smartphone.prototype = new Phone();
smartphone.prototype.constructor = smartphone;
smartphone.prototype.photo = function () {
  console.log("I can take a photo!");
};
const xiaomi = new smartphone("小米", 3000, "white");
console.log(xiaomi);
```

![图片](https://uploader.shimo.im/f/tKOEn7xnYnCW0LeB.png!thumbnail?fileGuid=CgwWHqgC8wc39Qdv)

## 使用 class 类继承

它的结果与构造函数继承相同

```javascript
class Phone {
  //构造方法
  constructor(brand, price) {
    this.brand = brand;
    this.price = price;
  } //父类成员属性
  call() {
    console.log("I can call u!");
  }
}
class smartphone extends Phone() {
  constructor(brand, price, color, size) {
    super(brand, price);
    this.color = color;
    this.size = size;
  }
  photo() {
    console.log("I can take a photo!");
  }
}
const xiaomi = new smartphone("小米", 3000, "white");
console.log(xiaomi);
```

- 上面代码第 14 行在子类中调用父类的构造方法做初始化
- 使用`super`方法，它在这里的作用等同于`Phone.call(this,brand,price)`
- ES6 的语法糖向 java 语法迈了一大步，将类的成员属性和类方法像 java 一样置于类内，不用在外部添加
- 子类通过`extends`直接继承

![图片](https://uploader.shimo.im/f/Ql014vYKUte8TGO9.png!thumbnail?fileGuid=CgwWHqgC8wc39Qdv)

## 子类重写父类

在子类声明一个和父类同名的方法，但是在子类中不可调用父类的**同名**方法

## class 中设置 getter 和 setter

- 当获取一个属性时，调用`get`方法,格式为`get 属性名(){}`
- 当设置一个属性时，调用`set`方法,格式为`set 属性名(newVal){}`
- get 方法必须`return`属性值，否则`undefined`
- set 方法必须有一个参数用以接收设置的新值

```javascript
class Phone {
  get price() {
    console.log("获取属性price");
    return 3000;
  }
  set price(newVal) {
    console.log(`设置属性price为${newVal}`);
  }
}
const xiaomi = new Phone();
console.log(xiaomi.price);
xiaomi.price = 5000;
```

### 应用场景

- get 方法将对象的动态属性进行封装，使得它能够随数据的变化而变化，类似于 computed
- 而 set 方法往往用于在属性值改变时判断是否合法
