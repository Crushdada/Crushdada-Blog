---
title: ES6-Promise：解决回调地狱
tags: es6
categories: Code
abbrlink: 28293
date: 2021-03-27 00:00:00
---


# Promise：解决回调地狱

另一个新的异步编程解决方案。语法上它是一个**构造函数**，用来封装异步操作并获取其成功或失败的结果

- Promise 构造函数：

```javascript
Promise(excutor){}
```
<!-- more -->

- promise 对象有两个属性，一个显示状态：fulfilled | rejected | pending(待定/初始状态)

## 使用它

- 实例化`Promise`对象

```javascript
const pro = new Promise(function (resolve, reject) {
  setTimeout(() => {
    let data = "Data";
    resolve(data);
  }, 1000);
});
```

- `resolve()`方法
  - 当**成功获取**到数据后，**在实例中**调用`resolve()`将数据传给处理`.then()`函数
  - 当**获取失败**后，**在实例中**调用`reject()`方法传参
- `.then`方法
- 它默认传入两个参数(可以只有一个成功的)，第一个用于接收`resolve()`传入的数据，第二个用于接收`reject()`传入的参数(一般为 Error 信息)

```javascript
pro.then(
  function (value) {},
  function (reason) {}
);
```

## Promise 封装读取文件

以往读取文件写法：

```javascript
const fs = require('fs');  //引入fs模块
fs.readFile('path/xx.md',(err，data)=>{   //调用方法读取
  if(err) throw err;  //如果失败，抛出异常
  console.log(data.toString()); //如果成功，输出
})
```

使用`Promise`封装(将其包裹)

```javascript
const pro = new Promise(function (resolve, reject) {
  fs.readFile("path/xx.md", (err, data) => {
    if (err) reject(err);
    resolve(data);
  });
});
pro.then(
  function (value) {},
  function (reason) {}
);
```

## Promise 封装 Ajax

一个 xml 请求：

```javascript
//创建对象
const xhr = new XMLHttpRequest();
//初始化
xhr.open("GET", "https://api.apiopen.top/getJoke");
//发送
xhr.send();
//绑定事件，处理响应结果
xhr.onreadystatechange = function () {
  if (xhr.readystate === 4) {
    //判断响应状态码 200-299
    if (xhr.status >= 200 && xhr.status < 300) {
      console.log(xhr.response);
    } else {
      console.error(xhr.status);
    }
  }
};
```

### 封装它！

```javascript
const p = new Promise((resolve, reject) => {
  const xhr = new XMLHttpRequest();
  xhr.open("GET", "https://api.apiopen.top/getJoke");
  xhr.send();
  xhr.onreadystatechange = function () {
    if (xhr.readyState === 4) {
      if (xhr.status >= 200 && xhr.status < 300) {
        resolve(xhr.response);
      } else {
        reject(xhr.status);
      }
    }
  };
});
p.then(
  function (value) {
    // console.log(value); //注意：需要将JSON转为js对象，才能使用其中属性
    let m = JSON.parse(value);
    console.log(m.message);
  },
  function (reason) {
    console.log(reason);
  }
);
```

## Promise.then 方法--进一步封装回调/异步

- 该方法的返回结果是`Promise`对象，其状态由回调函数的结果决定

该方法

- 如果回调函数中`return`的是非`Promise`属性，then 方法返回状态为成功，返回值为对象的成功值

如果是`Promise`对象 --

```javascript
const res = p.then((value) => {
  return new Promise((resolve, reject) => {
    resolve("ok");
  });
});
console.log(res);
```

![图片](https://uploader.shimo.im/f/DOK232Y52GQ4nzQb.png!thumbnail?fileGuid=wKPH6GKPqvvtRYXK)

- 在 return 函数中使用 reject 或 throw 一个 error，then 方法返回状态都是失败

```javascript
return new Promise((resolve, reject) => {
  reject("ok");
});
//抛出错误：
throw new error("error!!"); //还会报错
```

### ![图片](https://uploader.shimo.im/f/pyML8CbJOdBRyErr.png!thumbnail?fileGuid=wKPH6GKPqvvtRYXK)

![图片](https://uploader.shimo.im/f/rBocW1dNdLaUdnJA.png!thumbnail?fileGuid=wKPH6GKPqvvtRYXK)

![图片](https://uploader.shimo.im/f/viBzEd5QCaHrztPb.png!thumbnail?fileGuid=wKPH6GKPqvvtRYXK)

### then 方法中可以使用**链式调用**

```javascript
p.then((value) => {}).then((value) => {});
```

如此一步步地推进，有效解决回调地狱

## 实践

### Promise 封装读取多个文件

详见：[尚硅谷-ES6](https://www.bilibili.com/video/BV1uK411H7on?p=28&fileGuid=wKPH6GKPqvvtRYXK)

## Promise 对象 catch 方法

它是一个`then`方法的**语法糖，**当`then`方法不指定第一个参数时与之相同

用`promise实例.catch`替代它，专门用于处理错误

```javascript
p.catch(function (reason) {
  //p是Promise对象实例
  console.warn(reason);
});
```

参考教程：b 喊尚硅谷
