---
title: Vue3-初识

tags: Vue

categories: Code

abbrlink: 28427

date: 2021-04-18 00:00:00
---


# Vue3 和 vue2 的不同？

- Vue3 的不同之一在于数据获取。Vue3 中的反应数据（`Reactive Data`）是包含在一个反应状态（`Reactive State`）变量中，我们需要访问这个反应状态来获取数据值。
- Vue2 使用**选项类型 API**（`Options API`）对比 Vue3**合成型 API**（`Composition API`）
  - 旧的选项型 API 在代码里分割了不同的属性（`properties`）：`data`，`computed`属性，`methods`，等等。
  - 新的合成型 API 能让我们用方法（`function`）来分割，相比于旧的 API 使用属性来分组，这样代码会更加简便和整洁。
<!-- more -->

在 Vue3.0，我们就需要使用一个新的`setup()`方法，此方法在组件初始化构造的时候触发。

为了可以让开发者对反应型数据有更多的控制，我们可以直接使用到 Vue3 的反应 API（`reactivity API`）

使用以下三步来建立反应性数据:

从 vue 引入`reactive`使用`reactive()`方法来声名我们的数据为反应性数据使用`setup()`方法来返回我们的反应性数据，从而我们的`template`可以获取这些反应性数据

上一波代码，让大家更容易理解是怎么实现的。

```plain
import { reactive } from 'vue'
export default {
  props: {
    title: String
  },
  setup () {
    const state = reactive({
      username: '',
      password: ''
    })
    return { state }
  }
}
```

这里构造的反应性数据就可以被`template`使用，可以通过`state.username`和`state.password`获得数据的值。
参考文章：

- [带你体验 vue2 和 vue3 开发组件有什么区别--从零学习前端开发](https://zhuanlan.zhihu.com/p/139590941?fileGuid=3V6yJVjvqYgKpWyw)
- [vue2 与 vue3 的区别--星空之火@田兴](https://blog.csdn.net/weixin_43638968/article/details/108800361?fileGuid=3V6yJVjvqYgKpWyw)
