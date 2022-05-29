---
title: ES6-理解let与块级作用域
tags:
  - JavaScript
  - es6
categories: Code
abbrlink: 46537
date: 2021-03-26 00:00:00
---


# ES6-理解 let 与块级作用域

## 举个栗子

```javascript
for (var i = 0; i < 3; i++) {
  //此处i默认为var类型，不写亦可
  setTimeout(function () {
    console.log(i); //依次输出3,3,3
  }, 1000);
}
for (let j = 0; j < 3; j++) {
  setTimeout(function () {
    console.log(j); //依次输出0,1,2
  }, 1000);
}
```
<!-- more -->

输出：
![图片](https://uploader.shimo.im/f/LQY0Zofuo7k5OMmG.png!thumbnail?fileGuid=QwWdqvVp86RWTcVv)

这是为什么呢？这就涉及到**块级作用域**和**作用域链**

### 首先，我们需要了解--

- 函数声明时会确定且保存其相关作用域链，无关该函数于何处被调用或如何被调用，也就是说，当定义一个函数时，实际上是保存了一个作用域链。
- `setTimeout`方法**不会阻止循环**，而是会将延时调用的函数放到最后执行，**等待循环执行完毕后**，根据作为参数传入的**时延开始**倒计时，倒计时结束后将函数放入队列中执行

要证明这一点，我再举个栗子--

```javascript
for (let j = 0; j < 3; j++) {
  console.log("Crushdada");
  setTimeout(function () {
    console.log(j); //依次输出0,1,2
  }, 1000);
}
```

结果：
![图片](https://uploader.shimo.im/f/VTXdkgWJGBAsisHa.png!thumbnail?fileGuid=QwWdqvVp86RWTcVv)

    * 理解`setTimeout`异步，参考：
    * [JS异步执行之setTimeout 0的妙用](https://www.jianshu.com/p/91121cf5924f?fileGuid=QwWdqvVp86RWTcVv)
    * 深入理解JS单线程&异步任务：[setTimeout运行机制](https://blog.csdn.net/qingwenxiutong/article/details/52397676?fileGuid=QwWdqvVp86RWTcVv)

---

下面让我们来分别解析一下

### 两个循环的执行过程--

**var 声明的 for 循环：**

- **i 是一个全局变量，在全局作用域内**
- 上面提到`setTimeout`方法会在循环结束后执行

```javascript
var i = 0;
var i = 1;
var i = 2;
var i = 3; //退出循环后还会执行一次自加操作
{    //第一次循环
  function () {
    console.log(i);
  }  
}
{    //第二次循环
  function () {
    console.log(i);
  }
}
{    //第三次循环
  function () {
    console.log(i);
  }
}
```

- 每次调用函数时，JS 会沿着作用域链**逐层**地向外寻找”i“这个变量
- 上面说到作用域链的头部是当前函数(的活动对象)，因此 JS 会先看看**函数作用域内**有没有
- 没有，就会向外一层，发现**全局作用域**下有**变量 i**
- 但是由于 3 次循环结束后，**全局变量 i 的值已经被覆盖了**
- 因此，只会输出 i 最后的值 3

**let 声明的 for 循环：**

- **i 不是一个全局变量，具有块级作用域(花括弧内)，只在块级作用域内存在**

```javascript
   //第一次循环
{
  let j = 0;
  function () {
    console.log(j);
  }
}
   //第二次循环
{
  let j = 1;
  function () {
    console.log(j);
  }
}
   //第三次循环
{
  let j = 2;
  function () {
    console.log(j);
  }
}
```

- 每次执行到循环体中的`setTimeout`方法，该方法都会将调用的回调函数放入“任务队列中”，等待主线程(执行栈中)的事件全部执行完毕后，执行队列头的事件。
- 第一次循环后，任务队列中只有一个被`setTimeout`方法放入的回调函数，其作用域链中记录的是 i 的初始值`0`
- 由于 let 声明的变量只存在于块级作用域内，因此每一次**循环体执行完毕**都**会\*\***销毁\***\*该变量**，然后在 for 循环出的**新块**中**let 声明一个新的变量 j**，按 for 循环原本**既定的顺序**为其赋值，然后执行循环体
- 因此第二次循环时，任务队列中的回调函数的作用域链中，记录的是新创建的，重新被赋值为`1`的变量 i
- 正是由于**块级作用域**的**相互独立**，**互不影响**，才**不会覆盖\*\***j 的值，**就此，我有点理解为什么**let 能防止数据污染了\*\*(还有 es6 规定 let 不能重复声明这一点)
- 综上，输出结果为 0、1、2

参考文章：

- [关于 let 与块级作用域的一些理解](https://blog.csdn.net/m16041902/article/details/100170185?fileGuid=QwWdqvVp86RWTcVv)
- [尚硅谷-let 经典案例实践](https://www.bilibili.com/video/BV1uK411H7on?p=4&spm_id_from=pageDriver&fileGuid=QwWdqvVp86RWTcVv)
- [闭家锁：js 中的活动对象 与 变量对象 什么区别？](https://www.zhihu.com/question/36393048/answer/71879330?fileGuid=QwWdqvVp86RWTcVv)
