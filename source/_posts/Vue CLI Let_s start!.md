---
title: Vue CLI Let's start!
tags: Vue-Cli
categories: Code
abbrlink: 4168
date: 2021-03-23 00:00:00
---


# Vue CLI

Vue cli(Command Line Interface)--

是官方提供的**脚手架工具**，提供搭建好的一套利用**webpack**管理 vue 的**项目结构**
<!-- more -->

首先确认安装了[vue 以及 vue cli](https://cli.vuejs.org/zh/guide/installation.html?fileGuid=cKVPjg6ggwVTjD8X)

## Let's Start！

创建脚手架项目

在命令行或编译器终端输入：

```plain
vue create project_name
```

### 两种模式

选择配置模式--默认/手动

![图片](https://uploader.shimo.im/f/IByU0wkFln1VeGUG.png!thumbnail?fileGuid=cKVPjg6ggwVTjD8X)

默认模式配置较简单

若选择手动模式：

选择添加哪些配置 (按【**空格**】即可)

![图片](https://uploader.shimo.im/f/B62ZZqNToDtDGq3U.png!thumbnail?fileGuid=cKVPjg6ggwVTjD8X)

然后--

选择：是否安装 router

选择 CSS 预处理器

选择`ESlint / format`

选择检查配置(可全选)

- 检查后保存
- 检查到不符合规范后自动修复并提交(需要安装 git)

选择：是否将`babel`、`ESlint`等配置放到同一文件中

选择：是否保存以上所有选择，以便下次创建

### 好几个文件夹

**node_models**：存储相关依赖包

**public**：存储静态资源，放在该文件夹中的内容不会被`webpack`打包，`webpack`只会对其进行简单的复制粘贴整理，因此往往在其中存放永久不变的静态资源或`webpack`不支持的第三方库

**src**：存储代码

|----**_assets_**：存储项目自己的静态资源(图片/字体)，其中内容会被`webpack`处理、**压缩**

|----**c\*\*\***omponents\*\*\*：存储小组件，如公共组件

|----**_views_**：存储大组件，如页面级、路由级组件

|----**_router_**：存储 VueRouter 相关文件

|----**_store_**：存储 Vuex 相关文件

|----**_APP.vue_**：存储**根组件/根实例**

|----main**_.js_**：最顶层项目**入口**

### **打包和运行**

本地 server 演示：(打包到内存中)

```plain
cd project-name
npm run server
```

如何打包到磁盘中：

```plain
npm run build
```

打包完毕发现目录中多了一个 dist 文件夹，存放打包好的程序

## APP.vue 和 main.js

- 使用 Vue cli 开发项目，已经默认导入了 Vue.js 在 node_models 中，可以直接--在**JS**文件或代码段中**导入 vue**

```plain
import Vue from 'vue'
```

- web 开发一般会有一个 index.html 文件，作为根页面，但 Vue 推荐：将根页面也组件化，

在 src 文件夹下，创建**App.vue**，包括<template>、<script>、<style>三部分

```xml
<template> //结构
  <div id="#app"></div>
</template>
<script> //业务
  export default{
    name: "App"
  }
</script>
<style scoped> //样式,默认其中样式为全局样式，设置scoped属性使其只应用于当前组件

</style>
```

然后在**main.js**中导入该组件

```javascript
import Vue from 'vue'
import App from './App.vue'
new Vue({
  el: "#app"
  render: c => c(App) //使用render函数将传入的APP组件替换#App区域
})
```

- 创建实例对象但没有使用，eslint 可能会报错

在实例上方插入一行

```plain
/*eslint-disable no-new */
```

## 在 Vue cli 中使用 Vuex

在 store 文件夹中创建.js 文件

### 导入 Vuex

```javascript
import Vue from "vue"; //在导入vuex前需要先导入vue
import Vuex from "vuex";
```

### 创建 Vuex 对象

```javascript
import Vue from "vue";
import Vuex from "vuex";
Vue.use(Vuex); //必须使用vue的use方法引用插件，否则报错
const store = new Vuex.Store({
  state: {
    //state相当于data，用于保存数据/状态
    name: "hello",
  },
});
```

### 暴露该对象

```javascript
const store = new Vuex.Store({
  state: {
    //state相当于data，用于保存数据/状态
    name: "hello",
  },
});
export default store;
```

### 在使用它的 main.js 文件导入它

```javascript
import store from "store文件夹下的定义该对象的js文件相对路径";
```

### 将 store 对象绑定到实例

```javascript
new Vue({
  el: "#app",
  store, //将store对象绑定到实例
  render: (c) => c(App),
});
```

- 在 Vue cli 中使用 Router 等插件同理，需要导入插件，Vue.use()，然后按部就班配置，最后将 store 或 router 对象暴露出去，以供在其他需要的地方导入它了，然后使用
- Router，还需要导入想要配置路由的各**组件**

## 修改 webpack 配置

cli 默认配置好了 webpack，但有时候我们需要增加一些插件，或重命名输出的文件夹。

我们需要**修改 webpack 配置**

详见：[配置参考](https://cli.vuejs.org/zh/config/#vue-config-js?fileGuid=cKVPjg6ggwVTjD8X)

其中详细解释了 vue cli 对 webpack 的封装的各属性

**方法：**

创建`vue.config.js`文件，通过在其中设置`configureWebpack`属性来新增`webpack`设置

```javascript
module.exports={
  outputDir: 'bundle',
  configureWebpack:{   //在该对象中编写原生配置
    plugins: [
      new webpack.BinnerPlugin:{
        binner: "crushdada"    //指定授权信息
      }
    ]
  }
}
```
