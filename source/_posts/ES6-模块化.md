---
title: ES6-模块化
tags: es6
categories: Code
abbrlink: 44348
date: 2021-04-02 00:00:00
---


# ES6 模块化

模块化好处：

- 防止命名冲突
- 代码复用
- 高维护性

<!-- more -->

语法命令

- `export`：用于规定模块的对外接口，将一个模块暴露出去
- `import`：引入其他模块

举个栗子：

我有一个 a.html，一个 b.js，两个文件

想要在 a 中引入 b 文件中的模块

那么需要--先在 b 文件中暴露它，然后在 a 文件中引入它

## 动态和静态地 import

### 静态的 import

- 使用静态的 import  语句导入，无论是否声明了   严格模式  ，导入的模块都运行在严格模式下
- 在浏览器中，import  语句只能在声明了`type="module"`的`<script>`标签中使用。
  > **这样做的好处**
- 用静态  import  更容易从代码静态分析工具和[tree shaking](https://developer.mozilla.org/zh-CN/docs/Glossary/Tree_shaking?fileGuid=xGhqVY3QY8tWTxc8)中受益。
  - 诸如 webpack 等打包工具会**依赖**程序的**静态结构**，在打包时自动删除未引用的代码
- 静态型的  import  是初始化加载依赖项的最优选择
  > **这样做的坏处**

标准用法的 import 导入的模块是静态的，会使所有被导入的模块，在加载时就被编译，因此无法做到按需编译，降低首页加载速度

```javascript
//导入单个接口
import { person } from "./b.js";
//导入多个接口
import { animal, temp } from "./b.js";
//使用 ‘as’ 重命名导入的接口
import * as b from "./b.js"; //这也是通用的导入方式
//使用 ‘as’ 重命名导入多个接口
// import { person as p, animal as an } from "/modules/my-module.js";
console.log(animal.attr); //(2) ["eat", "drink"]
console.log(b.person); //{dosth: "play lol"}
console.log(person); //都可以直接使用
```

### 动态的 import

- 一个类似函数的动态  import()，它不需要依赖`type="module"`的`<script>`标签。
  - 在[script](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/script?fileGuid=xGhqVY3QY8tWTxc8)标签中使用`nomodule`属性，可以确保向后兼容。
- 在希望按照一定的条件或者按需加载模块的时候，使用动态`import()`是非常有用的。
- 但不要滥用动态导入，你可能在以下几种必要情况下，使用它
  > **可能会需要动态导入的场景**
- 当**静态导入时**，**代码的加载速度低**且**被使用的可能性很低**，或者并**不需要马上使用它**。
- 当**静态导入时**，**占用内存过大**且**被使用的可能性很低**。
- 当**被导入的模块**，**在加载时并不存在，需要异步获取时**
- 当**导入模块的说明符，需要动态构建时**。（静态导入只能使用静态说明符）
- 当**被导入的模块有副作用**（这里说的副作用，可以理解为**模块中有会直接运行的代码**/**全局的代码**），这些副作用只有在触发了某些条件才被需要时。
  - 原则上来说，模块不能有副作用，但是很多时候，你无法控制你所依赖的模块的内容

### 模块多种引入方式应用

#### 解构赋值--详见：[MDN-| import](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/import?fileGuid=xGhqVY3QY8tWTxc8)

## 暴露数据 export

- 除了在需要暴露的属性或数据前加上`export`关键字来**分别暴露**这一形式...
- 还可以通过一个接口(放在一坨)**统一暴露**出去 ↓

```javascript
const person = {
  dosth: "play lol",
};
const animal = {
  attr: ["eat", "drink"],
};
const temp = "Foo";
export { person, animal, temp };
```

### 默认暴露 export default

- `export default`是模块的**默认对外接口**，一个文件只能出现一次，暴露一个数据整体
  - 因此，在其他地方引入时不需要使用“{}”封装
  - 还可以省略`as`，直接重命名

```javascript
import { default as name } from "path/模块";
import name from "path/模块";
```

- export default 暴露的必须是**已经声明的数据**

# 使用 babel 转换 ES6=>ES5

有些浏览器(如 IE)尚未支持 ES6 模块化部分语法，因此我们需要使用 babel 对其进行转化，变为浏览器可以识别的语法（ES5）。提升其兼容性

## 工具：

- babel-cli：babel 命令行工具
- babel-present-env：预设包，进行代码转化
- browserify/webpack(开发级)：打包工具

详见：[尚硅谷：Babel 教程](https://www.bilibili.com/video/BV1uK411H7on?p=46&fileGuid=xGhqVY3QY8tWTxc8)
