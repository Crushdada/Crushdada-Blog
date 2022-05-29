---
title: JS面向对象？几种继承方式？原型继承？
tags: 面经
categories: Code
abbrlink: 32269
date: 2021-06-08 00:00:00
---


# 面向对象的三大特点：

继承、封装、多态

<!-- more -->

> 继承：

- 提到继承，就要涉及到**类的概念，我的理解：类就是一个或一些事物的共同属性和方法的一个抽象**
- 那么继承，就是表面意思，去继承他的父类所描述的那些属性和方法，并且允许它去扩展、重写等等
  > 封装

就是将系统模块装箱，隐藏内部实现，只暴露给外部调用接口

> 多态

就是多种状态，**接口的多种不同的实现方式**

也就是说，一个父类的方法或者接口，被多个子类继承并且重写了，这样，父类的一个方法就在子类中有了多种表现形式，就形成了多态

# JS 的面向对象

# 概述

- 面向对象是一种思想，或者说编程模式
- 它与面向过程不同，面向过程关注的是发展的过程，而面向对象关注的是参与者是谁，如何参与的？
  > > JS 面向对象机制的**前**(li)**世**(shi)**今(**yi)**生**(liu)
- 对于 JS 来说，一切皆对象。这是有一定的历史原因的--因为 JS 的作者 Brendan Eich 在当时开发 JS 时，面向对象的编程思想正泛滥
- 当时的浏览器非常初级，以至于 Brendan Eich 不想把 JS 写得那么正式/复杂，它只需要满足最简单的浏览器-用户的交互需求即可，比如--填写一个用户名
  > > Brendan Eich 的做法 1--new 构造函数
- 他参考了一下其他语言，发现都是通过 new 类的构造函数来实现的继承，然后--他就采用了一种简化的方式：抛弃类，允许在 JS 中直接 new 构造函数得到实例对象，以此实现继承。

_诶，有类我不写，就是玩~_

- 但是--这种方法有一个**缺点--无法共享属性和方法**，或者说这种方法创建的实例对象继承的只是值，而不是地址
  * 这意味着如果要继承的属性有 5 个，每个占内存 10，若需要创建 100 个实例对象，那么光是这些属性所消耗的内存就是：5*10*100，对性能很不友好
  * 除此之外，如果一个实例对象添加或修改一个属性，影响不到其他的实例对象。
  > 为了解决这个缺点--
  > Brendan Eich 的做法 2--引入 prototype 属性
- 为**构造函数**设置一个**prototype 属性，**专门用来**存放**实例对象们能够共享的属性和方法，那些不需要共享的属性和方法，就还是放在构造函数里面。
- 这样子，实例对象的属性和方法就被分成两种，**一种是不共享的，另一种是共享的的**。
  > new 关键字完全继承，
- 它其实是一个被封装的函数，其核心机制是**setPrototypeOf()**和**apply()**
- **当 new 一个实例对象时，将会\*\***通过调用 setPrototypeOf()“自动引用"prototype 对象的属性和方法，(获取到 prototype 对象里存放的属性和方法)\*\*
- **然后调用 apply()方法，(获取到构造函数中存放的属性和方法)**
  > > **这就是原型模式**
- 用于**创建实例对象**，实现了**高性能继承**。
- **在需要继承时，允许一个对象直接克隆一个已有的原型对象，以此快速地生成和它一样的新对象实例，所有\*\***对象实例会共享原型对象的所有属性和方法\*\*

---

> 明白了原型模式就很容易理解原型和原型链了--
> **原型和原型链**

## 原型 prototype

每个函数(构造函数)都有一个**prototype**属性，它本身是个引用，指向一个对象，叫做**原型对象**，其中包含了可以由**由同一个构造函数创建的所有实例对象**共享的属性和方法

_可以简单理解：原型就是一个模板，\*\*可以通过克隆它实现继承_

**\*Tips：\*\***一个构造函数(包括它自己)，和由它创建的所有实例对象都以引用的方式，共用一个原型对象，(因为在 JS 中，函数本身是对象)\*

> 隐式原型**proto**

- 每个**对象**都有一个**proto**属性,指向**它的构造函数的原型对象**
- **即：**它的值 === 它的构造函数的`[[Prototype]]`的值

我们举个栗子，就很好理解了--

```javascript
let constructor = function () {};
let obj = new constructor();
console.log(obj.__proto__ === constructor.prototype);
//true
```

**一个大坑**：“**proto**”这个属性的正确写法是**两边是各两个下划线**“\_”
ES6 之后更推荐使用`Object.getPrototypeOf/Reflect.getPrototypeOf`和`Object.setPrototypeOf/Reflect.setPrototypeOf`

**不推荐直接使用该属性：**

为什么？

- 因为**proto**前后的双下划线，说明它本质上是一个内部属性，而不是一个正式的对外的 API ，只是由于浏览器广泛支持，才被加入了 ES6 。
- 无论从语义的角度，还是从兼容性的角度，都不要使用这个属性，而是使用下面的 Object.setPrototypeOf()（写操作）、Object.getPrototypeOf()（读操作）、Object.create()（生成操作）代替。
  > Obj.constructor 属性

是另一个对象的属性，它指向该对象的构造函数

```javascript
function company() {
  this.name = " name";
  this.address = "address";
}
let obj = {};
Object.setPrototypeOf(obj, company.prototype);
console.log(obj.constructor === company); //true
// 和obj.__proto__联动一波，增进理解
console.log(obj.__proto__ === obj.constructor.prototype); //true
```

## 原型链

每个**对象**都有**proto**属性，来指向**它的构造函数的原型对象，**然后**原型对象**也是对象，也拥有**proto**属性...然后就这样一直往上层指，直到`null`(链的根部或者尾部就是 null)，就形成了一条**原型链**

> 原型链查询

当访问一个对象的属性时，如果该对象内部没有这个属性，那么就会去它的**proto**属性所指向的那个原型对象里找，如果还找不到，就继续往父级的构造函数的原型对象里找...直到原型链顶端 null

_这个过程和执行上下文、作用域链很像_

# JS 实现继承的几种方式？

## 1.构造函数+apply 继承

```javascript
function company() {
  this.name = " name";
  this.address = "address";
}
let obj = {};
company.apply(obj);
console.log(obj);
//{ name: ' name', address: 'address' }
```

> 只能继承构造函数里的属性,不能继承构造函数原型及原型链上的属性，如下 ↓

```javascript
function company() {
  this.name = " name";
  this.address = "address";
}
company.prototype.phone = 110;
let obj = {};
company.apply(obj);
console.log(obj.phone); //undefined
```

##

## 2.prototype 原型继承

原型继承更高效且节省内存，也更 JS

```javascript
function company() {
  this.name = " name";
  this.address = "address";
}
company.prototype.phone = 110;
let obj1 = {};
let obj2 = {};
Object.setPrototypeOf(obj1, company.prototype);
Object.setPrototypeOf(obj2, company.prototype);
console.log(obj1.phone === obj2.phone && obj1.phone === 110); //true
```

> 只能继承构造函数的原型对象(prototype)和原型链上的属性,不能继承构造函数内部的属性

```plain
console.log(obj1.name); //undefined
```

## 3.new 关键字类式继承

### 概述

- **JS 中其实是没有类的概念的**，所谓的类是构造函数模拟出来的。当我们使用 new 关键字的时候，或者 ES6 的 Class 关键字，就感觉“类”的概念很像 java。但其实，new 和 class 关键字底层实现机制还是基于**原型**的，它们都是语法糖。
  > 那么在 JS 中，是如何实现类继承的？
- 就是构造函数当做父类，然后通过 call 和 apply 方法，改变 this 的作用环境，使得子类能够获取到父类的各种属性。可以说 ES6 引入 call 和 apply 方法一部分原因就是为了更好地面向对象。

**注意：**

- **JS 函数中的 this 对象就像是一个函数的隐式参数，或引用**
- 实际上 new 关键字--是一个封装好的函数，其内部机制是上面第 1、2 个方法的组合,因此这种方式融合了二者的优点，能完全继承**构造函数内部属性+原型对象里的属性+原型链上属性**

```javascript
function company() {
  this.name = "Billie";
  this.address = "address";
}
company.prototype.phone = 110;
let obj1 = new company();
let obj2 = new company();
console.log(obj1.phone === obj2.phone && obj1.phone === 110);
console.log(obj1.name === obj2.name && obj1.name === "Billie");
// true
```

> 来看一个稍复杂点的栗子
> 在函数对象内用过 apply 调用父类的构造函数，使得自身获得父类(父级构造函数)的方法和属性\*\*\*\*

```javascript
var father = function () {
  this.age = 52;
  this.say = function () {
    alert("hello word");
  };
};
var child = function () {
  this.name = "bill";
  father.call(this);
};
var man = new child();
man.say();
```

在上面代码中 ，首先，new 关键字内部执行到-- let result = child.apply(man, null);
会调用 child 函数，然后将其中的 this 直接换成 man，就像这样 ↓

```javascript
var child = function () {
  man.name = "bill";
  father.call(man);
};
var man = new child();
man.say();
```

然后，执行到了 call 方法，同理，执行 father 方法并将其中的 this 换成 man--

```javascript
var father = function () {
  man.age = 52;
  man.say = function () {
    alert("xxx");
  };
};
```

然后，man 对象已经存在那些属性和方法了，因此直接调用 man.say()即可

## 或者--灵活运用 Object.create()方法、obj.constructor 属性

> Object.create()方法

- 逻辑：该方法会创建一个新对象，并使用传入的对象当做新创建的对象的**proto**，然后返回这个新对象
- 参数：对象
- 返回值：对象

因此，这可以是另外一种创建实例对象的方式，你可以**灵活运用**它

```javascript
function company() {
  this.name = "Billie";
  this.address = "address";
}
company.prototype.phone = 110;
let obj1 = Object.create(company.prototype);
//这样就和第2种继承方法一样了
console.log(obj1.__proto__ === company.prototype);
console.log(obj1.phone);
console.log(obj1.name);
//true
//110
//undefined
```

> obj.constructor 属性

而对于 constructor 属性，你可以**灵活运用**该方法来实现**构造函数之间的继承**(因为只有函数有这个属性)

```javascript
function Parent() {
  this.name = "parent5";
  this.play = [1, 2, 3];
}
function Child() {
  Parent.call(this);
  this.type = "child5";
}
// 产生一个中间对象隔离`Child`的`prototype`属性和`Parent`的`prototype`属性引用的同一个原型。
Child.prototype = Object.create(Parent.prototype);
// 给Child的原型对象重新写一个自己的constructor。
Child.prototype.constructor = Child;
```

**参考：**
[Javascript 继承机制的设计思想--阮一峰](http://www.ruanyifeng.com/blog/2011/06/designing_ideas_of_inheritance_mechanism_in_javascript.html?fileGuid=C9Yvj6WhckwwyXV3)

[JS 面向对象是什么？](https://www.cnblogs.com/shizk/p/9561997.html?fileGuid=C9Yvj6WhckwwyXV3)

[《JS 中 new 操作符做了什么？》--Crushdada's shimo Notes](https://shimo.im/docs/vR8Jkx8PYwvg8c39/?fileGuid=C9Yvj6WhckwwyXV3)

[对 JS 原型和原型链的理解--CSDN](https://blog.csdn.net/weixin_42614080/article/details/93413476?fileGuid=C9Yvj6WhckwwyXV3)

[JS 原型继承和类式继承](https://www.cnblogs.com/constantince/p/4754992.html?fileGuid=C9Yvj6WhckwwyXV3)

[JavaScript 中的 this 是什么？它到底做了什么？--思否](https://segmentfault.com/q/1010000039949202?fileGuid=C9Yvj6WhckwwyXV3)

[JS 继承的几种方式](https://blog.csdn.net/victor_ll/article/details/79498359?fileGuid=C9Yvj6WhckwwyXV3)

[js 中**proto**和 prototype 的区别和关系？](https://www.zhihu.com/question/34183746?fileGuid=C9Yvj6WhckwwyXV3)
