---
title: ES7-新特性：async&await
tags: es6
categories: Code
abbrlink: 21011
date: 2021-04-03 00:00:00
---


# ES7 新特性

- `Array.prototype.includes`:检查数组中是否包含某元素，返回 bool 值

```javascript
const arr = ["Foo", "Bar", "Crushdada"];
console.log(ar.includes("Foo")); //true
```

- ES7 允许用表达式计算乘方

<!-- more -->

```javascript
console.log(2 ** 10); //1024
console.log(Math.pow(2, 10)); //ES5方法
```

## `async`和`await`：新的异步编程解决方案

## async 函数

它像是一个 Promise 的语法糖,不必再局限于 new 一个 Promise，然后在里面穿透出数据给.then 方法的格式，但是，一般`async`是用来和`await`结合使用的！

- `async`函数的返回值为`promise`对象
- `promise`对象的结果由`async`函数执行的返回值决定

## await 表达式

- `await`必须写在`async`函数中
- 其右侧的表达式一般为`promise`对象
- 它返回的是`promise`成功的值
- `await`的`Promise`失败了，就会抛出异常，需要`try...catch`捕获它

```javascript
const p = new Promise((resolve, reject) => {
  resolve("got it!"); //reject("shibai");
});
async function fn() {
  try {
    let res = await p;
    console.log(res);
  } catch (rea) {
    console.log(rea);
  }
}
fn();
```

## async+await 读取多文件应用

**步骤：**

- **引入 fs 模块**：调用`readFile`方法
- **封装读取**：用不同的的函数读取，返回各自的`Promise`对象
- **声明一个 async 函数**：在其中使用`await`+`try...catch`统一处理

```javascript
function readFile1() {
  return new Promise(function (resolve, reject) {
    fs.readFile("../../test/1.txt", (err, data) => {
      if (err) reject(err);
      resolve(data);
    });
  });
}
function readFile2() {···}  //此处省略，和readFile1相比只改了路径
function readFile3() {···}  //同上
async function deal() {
  try {
    let res1 = await readFile1();
    let res2 = await readFile2();
    let res3 = await readFile3();
    console.log(res1.toString());
    console.log(res2.toString());
    console.log(res3.toString());
  } catch (rea) {
    console.log(rea); // console.log('错误！');
  }
}
deal();
```

- 若修改成一个错误路径“22.txt”，那么 catch 到的错误信息为

![图片](https://uploader.shimo.im/f/ITFWomY4zVXn8uf8.png!thumbnail?fileGuid=GpRGyCghW9PKXHwc)

- 你也可以不返回这个错误，仅返回“错误！”

![图片](https://uploader.shimo.im/f/un9ZTmMgfrbH5nNx.png!thumbnail?fileGuid=GpRGyCghW9PKXHwc)

## async+await 封装 Ajax！

- 写一个请求函数，返回一个 new 出来的 Promise 对象，在其其中发送 Ajax 请求
- 写一个 async 函数，用来调用上述函数并用 await 接收返回值

```javascript
function ajax(url) {
  return new Promise((resolve, reject) => {
    const x = new XMLHttpRequest();
    x.open("GET", url);
    x.send();
    x.onreadystatechange = () => {
      if (x.readyState === 4) {
        if (x.status >= 200 && x.status < 300) {
          resolve(x.response);
        } else {
          reject(x.status);
        }
      }
    };
  });
}
//使用promise.then的形式
// ajax("https://api.apiopen.top/getJoke").then(
//   (val) => {console.log(res);},
//   (rea) => {console.log(rea);}
// );
async function receive(url) {
  try {
    let res = await ajax(url);
    console.log(JSON.parse(res));
  } catch (rea) {
    console.log(rea);
  }
}
receive("https://api.apiopen.top/getJoke");
```
