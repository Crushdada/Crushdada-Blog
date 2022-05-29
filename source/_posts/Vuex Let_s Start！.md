---
title: Vuex Let's Start！
tags: Vue
categories: Code
abbrlink: 56859
date: 2021-03-06 00:00:00
---

<!-- more -->

# Vuex

[Vuex](https://vuex.vuejs.org/zh/guide/?fileGuid=6JwtVvCXtqGGGgC6)：Vue 配套的公共数据管理工具(响应式状态存储库)，用于保存共享的数据，整个程序中的任何组件都可访问和**提交数据变动**

**--可用于简单实现多级组件、兄弟组件数据传递**

## 使用：

1. **导入**vuex.js
2. 创建对象 vuex 对象(状态存储仓库 store)

```javascript
const store = new Vuex.Store({
  state: {
    //state相当于data，用于保存数据/状态
    count: 0,
  },
});
```

3. 在**祖先**组件**注册处**添加`store：store`

然后...

1. **访问共享数据**

该**祖先组件及其子孙组件**即可通过`this.$store.state.count`，访问**store**中的**state**中的**数据**

2. **修改共享数据**

由于组件复用，当共享数据变动导致错误时，不清楚是哪个组件发生了错误，就要逐一检查，因此不建议直接修改共享数据，而采用--

**提交数据变动**：在 vuex 对象中添加 mutation(突变)对象，与 state 同级，如下 ↓

```javascript
const store = new Vuex.Store({
  state: {
    //state相当于data，用于保存数据/状态
    count: 0,
  },
  mutations: {
    //mutations:突变,用于保存数据变动函数
    increment(state) {
      state.count++;
    },
  },
});
```

- 在`mutations`中定义数据变动函数，vuex 会默认为这些函数传入`state`，允许在其中使用 state.数据名改变数据状态
- 当需要数据改变时，使用`this.$store.commit('函数名')`调用`mutations`中对应的数据变动函数即可，可以理解为**显式**地**提交了数据变动**
- 这样的话，错误不在组件，而在这些数据变动函数中，因此直接到对应函数中追踪错误即可

3. getters

```javascript
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: "...", done: true },
      { id: 2, text: "...", done: false },
    ],
  },
  getters: {
    //相当于实例中的computed，第一次调用后会缓存
    doneTodos: (state) => {
      return state.todos.filter((todo) => todo.done);
    },
  },
});
```

vuex 也会将 state 传入，以供操作
