---
title: ES6-rest参数和扩展运算符
tags:
  - JavaScript
  - es6
categories: Code
abbrlink: 20794
date: 2021-03-29 00:00:00
---


# ES6 函数参数默认值

```javascript
function add(a, b, c = 10) {
  return a + b + c;
}
let result = add(1, 2);
result; //13
```
<!-- more -->

- 若未传参数，赋默认值
- 一般将设默认值的参数放在后面

**常用方法:**

与解构赋值结合使用，通过解构赋值的方式传参(详见解构赋值)，同样，也可为用于解构的变量设默认值

# rest 参数

- 是一种**形参格式**，替代`agruments`
- 格式为`...+形参名`
- 它会将接收到的用多个逗号分隔的实参，转化为**一个数组**
- 因此可以直接调用数组的 API：`filter`、`some`、`every`、`map`，提高了处理参数的灵活程度
- 当有多个参数，`rest`参数必须放在最后
- 当实参数量多于(传入的)形参，rest 会接收剩余的实参

```javascript
function date() {
  //ES5写法
  console.log(agruments);
}
date("浩宇", "Crush", "dada");
```

![图片](https://uploader.shimo.im/f/8NRKbCeJQuswwsQf.png!thumbnail?fileGuid=XrPXDTWVp88xyD99)

它是一个对象

```javascript
function date(...args) {
  //ES6写法
  console.log(args);
}
date("浩宇", "Crush", "dada");
```

![图片](https://uploader.shimo.im/f/31uxX5uf8TESgEXw.png!thumbnail?fileGuid=XrPXDTWVp88xyD99)

# 扩展运算符

- 写法与`rest`相同：`...+形参名`，但是是写在实参上
- 扩展运算符，顾名思义，用于**展开数组**
- 作用与`rest`参数相反，用于将**实参**【**数组**】转换为用逗号分隔的【**参数序列**】

## **应用：**

### 拼接字符串

```javascript
const arr1 = ["Crush", "dada"];
const arr2 = ["柠檬", "lemon"];
const combin = arr1.concat(arr2); //['Crush','dada','柠檬','lemon']
const combin = [...arr1, ...arr2]; //效果相同
```

[concat()方法](https://www.w3school.com.cn/jsref/jsref_concat_array.asp?fileGuid=XrPXDTWVp88xyD99)：用于拼接数组

### 数组拷贝

```javascript
const arr = ["a", "b", "c"];
const copy = [...arr];
```

### 将伪数组转化为真正的数组

```javascript
const divs = document.querySelectorAll("div");
const divArr = [...divs];
console.log(divs); //NodeList
console.log(divArr); //Array
```
