---
title: cookie和session和localstorage
tags: 面经
categories: Code
abbrlink: 32263
date: 2021-06-02 00:00:00
---


# 概述

> 为什么需要 cookie 和 Session？
> --由于 Http 协议的无状态性，要么在前端缓存，要么在后端维护--会话状态
<!-- more -->

Web 应用程序是使用 HTTP 协议传输数据的。**而 HTTP 协议是无状态的协议。一旦数据交换完毕，客户端与服务器端的连接就会关闭，再次交换数据需要建立新的连接。**这就意味着**服务器无法从连接上跟踪会话**。

**换句话说，如果不在传输的内容上做手脚的话，服务器无法区分不同的用户信息**

# **Session：会话控制**

与 Cookie 不同的是，它保存在服务器上。

## 它是如何实现的？

它会在服务端维护一个**用户会话状态(就是 Session)**，通过一个 StreamID 来区分不同用户，因此当你需要前后端的一个数据交互，前端发请求给服务端，就不用在请求中传重复的信息来区分不用用户的会话了，而是**把跟踪会话这个活给服务端做，它会在在处理新的请求时，根据一个 ID 去 Session 中查找对应用户的会话状态**。

#

# localStorage

- 与 Cookie 相比，拥有更大的存储容量
- 而 Cookie 每次请求都会被携带发送，浪费带宽。localStorage 不会
- localStorage 有自己封装好的方法，如 setItem、getItem 等，而使用 Cookie 的话，要么自己封装，要么引入第三方库
- cookie 的作用是与服务器进行交互，而 localStorage 仅仅是为了在本地“存储”数据
  > localStorage--
- 减少了网络流量：存储在本地，不用再向服务器请求数据，减少了请求次数
- 性能好：从本地读数据比请求服务器上快得多

## 记一次使用

- 朋友写了一个小练习--单页面+vue 实现 todolist，让我帮忙改成用 localStorage 存储，他的未完成和已完成的任务条目都是放在一个对象数组中的，然后每个对象有一个布尔类型的 done 属性区分二者，因此逻辑很简单--

我只需要将整个列表放在 localStorage 中，然后在 beforeCreated 钩子中从本地缓存读取它

```javascript
beforeCreate() {
    this.list = JSON.parse(localStorage.getItem('toDoList')) 
},
```

然后监听一下页面刷新/关闭事件：beforeunload

- 在其中将 list 存入本地缓存即可--
  - 需要注意的是，localStorage 只能**读取字符串**，因此需要借助 JSON.stringify 和 JSON.parse 来转换

```javascript
window.addEventListener("beforeunload", function (e) {
  localStorage.setItem("toDoList", JSON.stringify(app.list));
  // e.preventDefault(); // Chrome requires returnValue to be set
  //   e.returnValue = "hello";
});
```

## beforeunload 事件

- 当浏览器窗口关闭或者刷新时，会触发 beforeunload 事件
- 可在该事件中使用 e.preventDefault，这样点完 × 页面也不会立即关闭，而是弹出一个提示框，提示离开可能不会保存更改，并询问是否关闭，此时用户可以取消()
  - 当然也可像上面代码一样不询问
- 需要注意的是一个历史遗留问题：
- `Event.returnValue`属性在远古时期被用于--**指定\*\***beforeunload 触发的\***\*弹出框中的询问消息，**后来被弃用(浏览器不会显示在`beforeunload`事件中创建的提示)
- 后来，H5 规范建议使用 e.preventDefault()而非`Event.returnValue`来实现弹出确认框这一功能，然而，**不是所有浏览器都支持这么做**
  _ 火狐支持 preventDefault
  _ 而微软 Edge、chrome 则仍沿用 e.returnValue，尽管浏览器不会显示赋给它的任何值
  > 因此，为了网页兼容，如果你需要 beforeunload 事件触发这么一个弹出框，**最好将这两条语句都写上**

# 三者区别

> > 客户端与服务端与本地

- Cookie 是一种客户端跟踪会话的手段，用来弥补**HTTP 协议无状态**的这种不足。
- Session 是之后出现的，它的目的和 Cookie 一样，但它是用在**服务端的**
- localStorage 存储在本地
  > > 主动携带？
- cookie 数据始终在**同源\*\***的 http 请求中携带\*\*，ajax 默认不会携带
- session 和 localStorage 不会**自动**把数据发送给服务器，
  > > 存储数据量的限制
- cookie 存储的数量和字符数量都有限制，不同浏览器限制不同，一般允许每个域名下存几十个 Cookie，每个 Cookie 字符数量一般为 4k
- Session 和 localStorage 无限制
  > > 安全性
- cookie 不安全，任何人都可以查看存放在本地的 Cookie，并进行一个 Cookie 欺骗--通过 document.cookie
- Session 安全，(因为它是保存在服务端的吗)
  > > 服务器压力
- 当网站浏览或请求次数很多的时候，使用 Session 会对服务器有很大的压力。
- 而 Cookie 是在浏览器端的，localStorage 是在本地的，不存在对服务器的压力
  > > 生命周期
- 若不指定过期时间(默认情况下)，
  - Cookie 和 Session 在浏览器被关闭前会一直存在
    - (这种称为会话 cookie，一般保存在内存里)
- 当指定了过期时间
  _ Cookie 会被浏览器存储在硬盘里，因此关闭浏览器再打开 cookie 仍有效
  _ session：当它不活动一段时间之后会被销毁，这一段时间的长度是可以指定的 \* localStorage 长期有效，除非手动删除
  > > 作用域
- Cookie 和 localStorage 在所有同源窗口都共享
- Session 只在一个浏览器窗口共享(即使是同一个页面也不行)

**参考：**

[Cookie 和 Session 的使用和区别--南栀倾寒 You | 简书](https://www.jianshu.com/p/9a561b36e9f3?fileGuid=TJGgrgDjKGxyWkYQ)

[cookie 和 session 的详解与区别--测试开发喵 | 博客园](https://www.cnblogs.com/l199616j/p/11195667.html#_label0?fileGuid=TJGgrgDjKGxyWkYQ)
