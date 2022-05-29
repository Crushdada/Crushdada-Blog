---
title: Vue-Plugin详解
tags: Vue-Plugin
categories: Code
abbrlink: 10380
date: 2021-03-24 00:00:00
---


# Vue-Plugin

之前所用的`Vue.use`,是**注册插件**的一种方式，它也可以用来**注册组件，**这需要**先把组件封装成插件**

<!-- more -->

> **如何封装全局组件**

在`components`文件夹中写好组件名`.vue`文件，然后在将组件名暴露出去--

```javascript
export default {
  name: "组件名",
};
```

然后在 main.js 中(由于是全局组件)调用这个组件名来注册它--

```javascript
import 组件 from "路径";
Vue.component(组件.组件名, 组件注册名);
```

## 使用 Vue.use 注册

### 先将组件封装成插件

参考：[Vue-插件](https://cn.vuejs.org/v2/guide/plugins.html#ad?fileGuid=qxHW6DdrqXyHpyJg)

一般在企业开发中的，会会在`src`文件夹下新建`plugin`文件夹，专门存放自定义插件

> 在该文件夹中一个插件的结构

- **plugin**
  - loading（文件夹）示例组件名
    - index.js
    - Loading.vue（组件）

**Loading 文件夹即一个插件所占空间**

- 在`index.js`中
- 导入封装好的组件
- 使用`install`方法，在其中进行全局组件注册
- 将该方法暴露出去

```javascript
import Vue from "vue";
import Loading from "./Loading";
export default {
  install: function () {
    Vue.component(Loading.name, Loading);
  },
};
```

### 在需要的地方(main.js)导入

此时需要导入所用插件的 index.js 文件

```javascript
import Loading from "./plugin/loading/index";
```

### 注册插件

然后就可以使用`Vue.use`注册该插件了，调用`use()`方法，Vue 就会检查该插件是否已经注册，会调用`install()`方法

### 插件的另一种使用方法

其本质还是一个组件，需要在 HTML 代码段中使用`<组件名></组件名>`来**手动调用**

但是，当需要控制它的**显示和隐藏**时，或需要从接口动态渲染组件呢？

使用`Vue.extend + vm.$mount`组合

我们需要**手动添加--**

在封装插件的 地方

```javascript
import Vue from 'vue'
import Loading from './Loading'
export default{
  install: function(){
  ​  //根据组件生成构造函数
    let LoadingContructor = Vue.extend(Loading)
    //根据构造函数创建实例对象
    let LoadingInstance = new LoadingContructor()
    //创建一个标签
    let oDiv = document.createElement('div')
    //将创建好的标签添加到页面上
    document.body.appendChild(oDiv)
    //将创建好的实例对象挂载到创建好的元素上
    LoadingInstance.$mount(oDiv)
  }
}
```

### 为插件传参

`Vue.use()`提供了一个可选的对象参数--它会将该对象传递给插件的 install()方法

可以在该对象中将数据传给插件，用来**在外部改变插件内容**

```javascript
Vue.use(Loading, {
  title: "Hello",
});
```

在 install 中接收

```javascript
export default{
  install: function(Vue, options){
  ​  //根据组件生成构造函数
    let LoadingContructor = Vue.extend(Loading)
    //根据构造函数创建实例对象
    let LoadingInstance = new LoadingContructor()
    //实例对象是根据组件创建的，因此它能够访问到其中的属性，如LoadingInstance.title
    //...
  }
}
```

现在，我们能够使用外部传入的`options`对象，也能够访问到组件中的属性
就可以实现**外部控制**了,即在注册插件时指定插件中属性的值

首先我们需要保证传入数据合法，

```javascript
if (options && options.title !== null && options.title !== undefined) {
  LoadingInstance.title = options.title;
}
```

### 控制显示与隐藏

既然能够指定其属性的值，那么指定 v-if 或 v-show 的值即可

tips:

- 定义全局方法：`Vue.方法名 = function(){}`
- 调用全局方法：`Vue.方法名()`

- 定义实例方法：`Vue.prototype.$方法名=function(){}`
- 调用实例方法：`this.$方法名()`

### 小结

封装组件就是为了方便为其增加一些扩展
