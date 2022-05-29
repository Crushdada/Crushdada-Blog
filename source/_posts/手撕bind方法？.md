---
title: JavaScript-手撕bind方法？
tags: JavaScript
categories: Code
abbrlink: 32259
date: 2021-05-18 00:00:00
---



> > 首先，它是函数的一个方法，我们需要将其--

# 1.挂载到 Function 的原型链上
<!-- more -->
```javascript
Function.prototype.mybind =...
//这样，所有继承自Function的函数就能够使用.操作符来访问mybind了！
//PS：因为JS原型式继承
```

> > 然后，让我们先看看原生 JS 的 bind 方法有哪些行为--

# 2.调用函数时改变 this 指向

> > 让调用该方法的函数的 this 指向传入的第一个参数

我们可以**借助 apply**方法实现

```javascript
Function.prototype.mybind = function (context) {
  this.apply(context);
};
let obj = {
  name: "Crushdada",
};
let fn = function (params) {
  console.log(this.name);
};
fn.mybind(obj); //Crushdada
```

# 3.返回一个匿名的绑定函数

> > **注意两点：**

- 由于我们返回了一个绑定函数(匿名函数)，则在调用时需要在调用语句后面再**加一个圆括号**
- 与此同时，由于匿名函数中的 this 指向 window/global，我们需要**使用箭头函数或**者手动**保存**一下指向 mybind 中指向调用者 fn 的**this**
  - _此处使用箭头函数_

```javascript
Function.prototype.mybind = function (context) {
  return () => this.apply(context);
};
let obj = {
  name: "Crushdada",
};
let fn = function (params) {
  console.log(this.name);
};
fn.mybind(obj)(); //Crushdada
```

# 4.支持柯里化传递参数

**个人理解**：相比“允许传入参数”这种说法，形容为“**传递参数**”更贴切，bind 方法作为一个中间方法，会**代收参数后再传递给它返回的匿名绑定函数，**其返回一个匿名函数这一点，**天然支持柯里化**(*可能是 ES6 引入它的初衷之一)，*因为这样就允许我们在调用 bind 时传入一部分参数，在调用其绑定函数时再传入剩下的参数。然后它会在接收完第二次传参后再 apply 执行调用 bind 的那个方法

- 实现柯里化的逻辑很简单，仅仅需要在 mybind 中接收一次参数，然后在绑定函数中接收一次参数，并将二者拼接后一起传给 mybind 的调用方法使用即可
  > > 下面，实现传参&柯里化！
- 若使用的是普通函数，要处理参数，由于 arguments 为类数组，slice 为 Array 方法，故先在原型链上调用然后 call 一下 \* 第一个参数为 this 新的指向，不是属性，故 slice 掉它
  > > 使用箭头函数能极大简化代码
  > > 下面我们改亿点点细节！
- 使用箭头函数（Array Function）没有 arguments 属性，因此使用**rest 运算符**替代处理
- 在拼接 args 和 bindArgs 时使用扩展运算符替代 concat
- _不得不说 ES6 引入的 rest 运算符、扩展运算符在处理参数这一点上提供了极大的便利_

```javascript
Function.prototype.mybind = function (context, ...args) {
  return (...bindArgs) => {
    //拼接柯里化的两次传参
    let all_args = [...args, ...bindArgs];
    //执行调用bind方法的那个函数
    let call_fn = this.apply(context, all_args);
    return call_fn;
  };
};
let person = {
  name: "Crushdada",
};
let getInfo = function (like, fav) {
  let info = `${this.name} likes ${like},but his favorite is ${fav}`;
  return info;
};
//anonymous_bind:mybind返回的那个匿名的绑定函数
let anonymous_bind = getInfo.mybind(person, "南瓜子豆腐");
let info = anonymous_bind("皂角仁甜菜"); //执行绑定函数
console.log(info);
//Crushdada likes 南瓜子豆腐,but his favorite is 皂角仁甜菜
```

## 箭头函数不能作为构造函数！

## 需要用普通函数重写 mybind

写到支持柯里化这一步，bind 方法还是可以使用箭头函数实现的，而且比普通函数更加简洁

但是想要继续完善它的的行为，就不能用继续用 Arrow Function 了，因为箭头函数不能被 new！，要是尝试去 new 它会报错：

```javascript
anonymous_bind is not a constructor
```

> > 笔者也是写到这才想起箭头函数这个机制的。那么下面我们需要用普通函数重写 mybind
> > 不过也很简单，只需要手动保存一下 this 即可。就不再贴出改动后的代码了。直接看下一步

# 5.支持 new 绑定函数

bind 的一个隐式行为：

- **它返回的绑定函数允许被 new 关键字调用**，但是，**实际被作为构造器的是调用 bind 的那个函数！！！**
- **且 new 调用时传入的参数照常**被传递给调用函数。
  > > 逻辑

实现这一步的逻辑也较为简单，我们类比一下和一般调用 new 时的区别--

- new 一个**普通函数**：按理来说生成的**实例对象的构造函数**就**是那个\*\***普通函数\*\*
- new 一个**绑定函数**：生成的**实例对象的构造函数**是**调用 bind 的那个函数**

主要需要我们写的逻辑有：

1. 判断是否是 new 调用
2. 让**getInfo 函数**中的 this 指向--new 中创建的实例对象 obj
   1. 就是把 getInfo 函数里的 this 换成 obj，以使 obj 获取到其中的属性
   2. 可以借助 apply 方法
3. 判断**getInfo 函数**是否返回一个对象，若是，则返回该对象，否则返回 new 生成的 obj
   > > 至于为什么这么写，就需要你先弄懂 new 关键字实现的机制了，我的笔记链接附在文末
   > > 下面，实现它！

```javascript
Function.prototype.mybind = function (context, ...args) {
  let self = this;
  return function (...bindArgs) {
    //拼接柯里化的两次传参
    let all_args = [...args, ...bindArgs]; // new.target 用来检测是否是被 new 调用
    if (new.target !== undefined) {
      // 让调用mybind的那个函数的this指向new中创建的空对象
      var result = self.apply(this, all_args); // 判断调用mybind方法的那个实际的构造函数是否返回对象，没有返回对象就返回new生成的实例对象obj
      return result instanceof Object ? result : this;
    } //如果不是 new 就原来的逻辑 //执行调用bind方法的那个函数
    let call_fn = self.apply(context, all_args);
    return call_fn;
  };
};
let person = {
  name: "Crushdada",
};
let getInfo = function (like, fav) {
  this.dear = "Bravetata";
  let info = `${this.name} likes ${like},but his favorite is ${fav}`;
  return info;
};
//anonymous_bind:mybind返回的那个匿名的绑定函数
let anonymous_bind = getInfo.mybind(person, "南瓜子豆腐");
let obj = new anonymous_bind("皂角仁甜菜"); //执行绑定函数
console.log(obj); //{ dear: 'Bravetata' }
console.log(obj.name); //undefined
```

> > 解释一下以上代码：

**第一个逻辑**

- new 内部有类似这样一条语句：Con.apply(obj, args)
- 其中 Con 是 new 的那个构造函数，obj 是最后要返回的实例对象
- 当我们 new 上面 mybind 中 return 的那个绑定函数时
- Con 就是该绑定函数
- 当 Con.apply(obj, args)执行，
- 调用绑定函数并将其中的 this 换成 obj
- 然后程序就进入到了该绑定函数中--
- 判断确实是 new 调用的
- 执行 self.apply(this, all_args);
- 这条语句就相当于 getInfo.apply(obj, all_args)
- 这样就达成我们的目的了！--让 getInfo 成为 new 生成的实例对象的实际构造器

**第二个逻辑**

- new 关键字会判断构造函数本身会不会返回一个**对象**
- 如果会，则直接返回这个对象当做实例，否则正常是返回那个 new 生成的 obj 当做实例对象
- 那么--我们在第一个逻辑里已经调用了实际构造器--getInfo
- 接下来我们直接判断一下调用的结果，即它是否 return 一个对象，然后 return 给 new 做最终的 return 即可
  > 此外：可以看到，当 new mybind 返回的绑定函数时，obj 没有获取到 person.name 属性，为 undefined。也就是说--

## 此时，bind 改变 this 指向的行为会失效

> > 看个栗子，这样清楚一点

```javascript
var value = 2;
var foo = {
  value: 1,
};
function bar(name, age) {
  this.habit = "shopping";
  console.log(this.value);
  console.log(name);
  console.log(age);
}
bar.prototype.friend = "kevin";
var bindFoo = bar.bind(foo, "daisy");
var obj = new bindFoo("18");
// undefined
// daisy
// 18
console.log(obj.habit);
console.log(obj.friend);
// shopping
// kevin
```

尽管在全局和 foo 中都声明了 value 值，最后依然返回了 undefind，说明绑定的 this 失效了，

> > 这是为什么呢？

如果大家了解 new 的模拟实现，就会知道了--

new 是 JS 模拟面向对象的一个关键字，它的目的之一是实现继承，它要去继承构造函数(类)之中的属性，那么 new 关键字是怎样去实现的呢？它在内部应用了**类似**这样一条语句：

```javascript
Con.apply(obj, args); //Con是new 的那个构造函数
```

new 关键字会先声明一个空对象 obj，然后将构造函数的 this 指向这个对象

> > 这样做会发生什么--

- 如果构造函数中设置了一些属性，如：this.name = xx;
- 那么就相当于将 this 换成了 obj，变成：obj.name = xx;
- obj 就继承到了构造函数的属性！！
- obj 就是最后会返回的实例对象

详见：[《JS 中 new 操作符做了什么？》--Crushdada's Notes](https://shimo.im/docs/vR8Jkx8PYwvg8c39/?fileGuid=y6HVHHPgG6gy9pdX)

> > 让我们回到为什么 this 会失效这一问题上
> > 了解完 new 关键字的相关实现，我们已经得到答案了--

new 完绑定函数后，绑定函数内部的**this 已经指向了 obj，而 obj 中没有 value 这个属性**，当然就返回 undefined 了

# 6.支持原型链继承

实际上这一步是对绑定函数内重写 new 方法的一个补充--

因为 new 方法本来就支持原型链继承

> > 逻辑

那么我们只需要--

让 new 的实例对象 obj 的原型指向实际构造器 getInfo 的 prototype 即可

```javascript
Object.setPrototypeOf(this, self.prototype);
```

> 规范化/严谨性

可以为 mybind 方法加上一个判断，调用者必须是一个函数,否则抛出 TypeError--

```javascript
if (
  typeof this !== "function" ||
  Object.prototype.toString.call(this) !== "[object Function]"
) {
  throw new TypeError(this + " must be a function");
}
```

> 一个疑问？

我们模拟实现 bind 方法，终归是通过 apply 实现的。而它源码是如何实现的，对于我来说就像一个黑盒。也就是说：不用 apply，它是如何实现？

# 7.究极版本--不借助 apply 实现

百度的各种版本大都借助 apply 实现的，不过很幸运在思否找到了答案--[JS bind 方法如何实现？](https://segmentfault.com/q/1010000040026250?_ea=132549597&fileGuid=y6HVHHPgG6gy9pdX)

答者给出的替代 apply 的方法很简单：

- 调用 bind 方法的那个函数--即要改变 this 指向的那个函数：caller
- 要让 caller 的 this 指向：context

那么我们只需要--

- 将 caller 作为一个对象方法挂载到 context 上：context.callerFn = caller
  - _上面那句代码中，属性名"callerFn"是自定义的_

这样，当执行该句时，相当于 context 调用了 caller 函数，那么 caller 函数中的 this 自然就指向其调用者 context 了。

以上，就替代了 apply 在本例中的核心功能--**调用函数同时改变 this 指向**

此外，为提高代码性能，用完 callerFn 后就删掉它

```javascript
context.__INTERNAL_SECRETS = func;
try {
  return context.__INTERNAL_SECRETS(...args);
} finally {
  delete context.__INTERNAL_SECRETS;
}
```

> > 将 apply 替换为以上代码，就得到最终版了

# 最终版

```javascript
FFunction.prototype.mybind = function (context, ...args) {
  if (
    typeof this !== "function" ||
    Object.prototype.toString.call(this) !== "[object Function]"
  ) {
    throw new TypeError(this + " must be a function");
  }
  let self = this; //这里的this和self即：调用mybind的方法--fn()
  context.caller2 = self;
  return function (...bindArgs) {
    let all_args = [...args, ...bindArgs]; //new调用时，this被换成new方法最后要返回的实例对象obj
    if (new.target !== undefined) {
      try {
        this.caller = self;
        var result = this.caller(...all_args);
      } finally {
        delete this.caller;
      }
      Object.setPrototypeOf(this, self.prototype);
      return result instanceof Object ? result : this;
    } //当不是new调用时，this指向global/window(因为匿名函数返回后由全局调用)
    try {
      var final_res = context.caller2(...all_args);
    } finally {
      delete context.caller2;
    }
    return final_res; //调用mybind的那个函数[可能]有返回
  };
};
```

# 其他知识点：

## Array.prototype.slice.call()

- 接收一个字符串或**有 length 属性**的**对象**
- 该方法能够将**有 length 属性**的**对象**或字符串**转换为数组**
  > 因此像是 arguments 对象这样拥有 length 属性的类数组就可以使用该方法转换为真正的数组
  > JS 中，只有 String 和 Array 拥有.slice 方法，对象没有。

```javascript
let slice = (arrlike) => Array.prototype.slice.call(arrlike);
var b = "123456";
let arr = slice(b);
console.log(arr);
// ["1", "2", "3", "4", "5", "6"]
```

## arr.slice()方法

- **返回一个新的数组对象**，这一对象是一个由`begin`和`end`决定的原数组的**浅拷贝**（**包括**`begin`，**不包括**`end`）。
  - 接收的参数--`begin、end`是数组 index
  - 原始数组不会被改变。

```javascript
const animals = ["ant", "bison", "camel", "duck", "elephant"];
console.log(animals.slice(2));
// expected output: Array ["camel", "duck", "elephant"]
console.log(animals.slice(2, 4));
// expected output: Array ["camel", "duck"]
```

## 逻辑或“||”

**a || b :**

- 若运算符前面的值为 false，则返回后面的值
- 若 true，返回前面的值

## js 中逻辑值为 false 的 6 种情况

> > 当一个函数拥有形参，但调用时没有传实参时，形参是 undefined,会被按 false 处理

```javascript
function name(params) {
  console.log(params); //undefined
}
name();
console.log(undefined == false); //false
console.log(undefined || "undefined was reated as false");
//undefined was reated as false
```

> > 会被逻辑或运算符当做 false 处理的总共 6 个--

**0、null、""、false、undefined  或者  NaN**

**参考：**

[JavaScript 深入之 bind 的模拟实现--掘金](https://juejin.cn/post/6844903476623835149?fileGuid=y6HVHHPgG6gy9pdX)

[js 实现 bind 的这五层，你在第几层？--蓝色的秋风 | 思否](https://segmentfault.com/a/1190000039980447?_ea=131978111&fileGuid=y6HVHHPgG6gy9pdX)

[《JS 中 new 操作符做了什么？》--Crushdada's Notes](https://shimo.im/docs/vR8Jkx8PYwvg8c39/?fileGuid=y6HVHHPgG6gy9pdX)
