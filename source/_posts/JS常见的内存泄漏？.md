---
title: JS常见的内存泄漏？
tags: 面经
categories: Code
abbrlink: 32267
date: 2021-06-06 00:00:00
---


# **一、什么是内存泄漏**

一块内存不再被应用程序所使用，但这块内存却没有返还给操作系统或空闲内存池。

# **二、几种常见的内存泄漏**

## 1、意外的全局变量
<!-- more -->

一个未声明变量的引用会在全局对象中创建一个新的变量。

比如：你想创建一个拥有函数作用域的局部变量，但是你忘了添加 var 来声明它，这时候就相当于声明了一个全局变量，就是为 window 对象加了一个属性。

另外一种偶然创建全局变量的方式如下：

就是在最外层函数中使用 this，如果在全局作用域下调用它，this 指向的就是 window

```plain
function foo() {
    this.variable = "potential accidental global";
}
foo();
```

## 2、闭包引起的内存泄漏

闭包可以使变量常驻内存，起到一个缓存的作用，但如果使用不当就会在成内存泄漏

```plain
var theThing = null;
var replaceThing = function () {
  var originalThing = theThing;
  var unused = function () {
    if (originalThing)
      console.log("hi");
  };
  theThing = {
    longStr: new Array(1000000).join('*'),
    someMethod: function () {
      console.log(someMessage);
    }
  };
};
setInterval(replaceThing, 1000);
```

上面代码中，每次调用`replaceThing`时，`theThing`都会得到新的包含一个大数组和新的闭包（`someMethod`）的对象。
同时，没有用到的那个变量持有一个引用了`originalThing`（`replaceThing`调用之前的`theThing`）闭包。

关键的问题是每当在同一个父作用域下创建闭包作用域的时候，这个作用域是被共享的。在这种情况下，`someMethod`的闭包作用域和`unused`的作用域是共享的。

`unused`持有一个`originalThing`的引用。尽管`unused`从来没有被使用过，`someMethod`可以在`theThing`之外被访问。

而且`someMethod`和`unused`共享了闭包作用域，即便`unused`从来都没有被使用过，它对`originalThing`的引用还是强制它保持活跃状态（阻止它被回收）。

当这段代码重复运行时，将可以观察到内存消耗稳定地上涨，并且不会因为 GC 的存在而下降。

本质上来讲，创建了一个闭包链表（根节点是`theThing`形式的变量），而且每个闭包作用域都持有一个对大数组的间接引用，这导致了一个巨大的内存泄露。

## 3、DOM 之外的引用

- 指在 JS 中获取节点之后，保存在某一数据结构之中，如 JSON 字典或数组，此时 dom 就有了两个引用，当我们不再需要那些节点时，如果不把存储在数据结构之中的 DOM 对象也删掉，就会造成内存泄漏
- 还有就是当获取了一个子节点时，它会记录其父节点的相关依赖，不将其删掉的话，父节点的内存也得不到释放

```plain
var elements={
    button: document.getElementById("button"),
    image: document.getElementById("image"),
    text: document.getElementById("text")
};
function doStuff(){
    image.src="http://some.url/image";
    button.click():
    console.log(text.innerHTML)
}
function removeButton(){
    document.body.removeChild(document.getElementById('button'))
}
```

## 4、被遗漏的定时器和回调函数

```plain
var someResouce=getData();
setInterval(function(){
    var node=document.getElementById('Node');
    if(node){
        node.innerHTML=JSON.stringify(someResouce)
    }
},1000)
```

上面代码中，  如果 id 为 Node 的元素从 DOM 中移除, 该定时器仍会存在, 同时, 因为回调函数中包含对 someResource 的引用, 定时器外面的 someResource 也不会被释放。
**三、怎样避免内存泄漏**

1）减少不必要的全局变量，或者生命周期较长的对象，主动对无用的数据进行垃圾回收；

2）注意程序逻辑，避免“死循环”之类的 ；

3）避免创建过多的对象   原则：不用了的东西要及时归还。

**参考：**

[JS 中四种常见的内存泄漏](https://blog.csdn.net/weixin_30248399/article/details/98228624?fileGuid=gVCtxkK9gHwXryxJ)
