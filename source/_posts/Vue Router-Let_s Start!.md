---
title: Vue Router Let's Start!
tags: Vue-Router
categories: Code
abbrlink: 40375
date: 2021-03-19 00:00:00
---


# Vue Router

## What's it？

[Vue Router](https://router.vuejs.org/zh/guide/#javascript?fileGuid=GqVwtWj8c8TtygCK)即**路由**，它的作用和 v-if、v-show 一样，用于**切换组件显示，但是--**

- v-if、v-show 是根据标记切换 (true/false)
- **Vue Router**是根据哈希切换 (#/xxx)
  - 更强的是，它能够在切换时传参
<!-- more -->

## Start

### 导入 Vue Router

注意：在引入 Router 前需先引入 Vue.js

```xml
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
```

### 定义或导入组件

### 定义路由规则，建立 "路由-组件" 映射

```javascript
const routes = [
  //数组中每一个对象是一条路由规则
  { path: "/foo", component: Foo }, //Foo是组件名
  { path: "/bar", component: Bar },
];
```

### 根据定义的规则创建路由对象 router

为路由对象配置 routes，即：将上面自定义的路由规则赋给 routes 属性，用于指定切换规则

```javascript
const router = new VueRouter({
  routes, // (ES2015缩写语法) 相当于 routes: routes
});
```

### 挂载到 Vue 根实例

将创建完的路由对象挂载到 Vue 实例上

为实例添加 router 属性(与 data 同级)，将路由对象赋值给它

```javascript
const app = new Vue({
  router, // (ES2015缩写语法) 相当于 router: router
}).$mount("#app");
```

## <router-link>路由导航

最基础的方法--使用 a 标签修改 url Hash 值，当点击时跳转到其他组件，但 Vue 提供了更规范和专业的方法--使用`<router-link>`

```xml
<div id="app">
  <!-- 使用 router-link 组件来导航. -->
  <!-- 通过传入 `to` 属性指定链接. -->
  <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
  <!-- 设置tag属性指定渲染成什么元素 -->
  <router-link to="/foo">Go to Foo</router-link>
  <router-link to="/bar" tag="button">Go to Bar</router-link>
  <!-- 路由出口 -->
  <!-- 路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
</div>
```

## <router-link>激活样式

`<router-link>`激活时，vue 会为其添加一个样式类：`router-link-active`，允许**重写该样式类**来自定义它的选中样式

更专业的做法--

在路由对象中添加一个键值对`linkActiveClass : "类名"`，指定它的激活样式，然后去实现它即可

## 路由重定向

```javascript
const routes = [
  { path: '/', redirect: '/foo' }
  { path: '/foo', component: Foo }, //Foo是组件名
  { path: '/bar', component: Bar }
]
```

可在**路由-组件映射**中添加重定向规则
当 url 的 Hash 值是根目录时，即"/"，重定向到显示 Foo 组件，就规定了初始展示的组件

## Vue Router 传递参数

当将 vue Router 挂载到实例后，就可以通过`vue.$route`访问路由对象，然后通过路由对象拿到传递的参数

**两种方法：**

### 通过 URL 参数--

    * 在<router-link>的to属性值指定URL同时，以**?+参数键值对**的形式传参，如下↓

```xml
<router-link to="/foo?id=10&name=David">
  Go to Foo
</router-link>
```

    * Vue Router会将传递的参数保存在路由对象的**query**中
    * 在组件的生命周期函数中通过`this.$route.query.参数`获取--

```javascript
const Foo = {
  template: "#foo",
  created: function () {
    console.log(this.$route.query.id);
  },
};
```

### 通过占位符--

    * **路由规则中**，使用**/ :key/ :key**，**指定占位符**

```javascript
const routes = [
  { path: "/foo/:id/:name", component: Foo }, //Foo是组件名
  { path: "/bar", component: Bar },
];
```

    * **指定Hash时**，使用**/value/value**，**传值**

```xml
<router-link to="/foo/7/David">
  Go to Foo
</router-link>
```

    * Vue Router会将传递的参数保存在路由对象的**params**中
    * 通过`this.$route.params.参数`获取

### **两种方法的区别：**

    * 保存的地方不同，一个是query，另一个是params
    * query更加类似于我们ajax中get传参，params则类似于post，前者在URL地址栏中显示参数，后者则不显示

## 嵌套路由

详见：[Vue Router-嵌套路由](https://router.vuejs.org/zh/guide/essentials/nested-routes.html?fileGuid=GqVwtWj8c8TtygCK)

需要将子路由嵌套在根路由(父级路由规则中)

在路由对象的参数中使用`children`配置子路由，格式沿用之前的形式 ：

```javascript
const router = new VueRouter({
  //此处将路由规则直接写入路由对象
  routes: [
    {
      path: "/Foo/:id",
      component: User,
      children: [
        {
          path: "profile", //不用写根路由和"/"，Vue Router会自动为子路由拼接
          component: UserProfile,
        },
        {
          path: "posts",
          component: UserPosts,
        },
      ],
    },
  ],
});
```

## 命名视图

- 像组件插槽类似，`<router-view>`是一个用来填的坑，当根据路由规则匹配到对应的路由时，将组件渲染在这个**路由出口** \* 若有多个路由出口，而无路由导航`<router-link>`,则会像匿名插槽一样同时渲染多个匹配到的那个组件
  > **嵌套路由视图<router-view>**
- **根路由**控制的区域(实例或组件)中的**路由出口**会渲染**最高级路由**匹配到的组件。
- 同样地，一个被渲染的组件内部(template 中)同样可以嵌套自己的`<router-view>`。例如，在  User  组件的模板添加一个  <router-view>：

```plain
const User = {
  template: `
    <div class="user">
      <h2>User {{ $route.params.id }}</h2>
      <router-view></router-view>
    </div>
  `
}
```

> 实现同一路由下渲染多个组件

需求即：改变路由-组件映射关系，变为一个路由映射多个组件

**方法：**

```javascript
const routes = [
  {
    path: "/", //在跟路由下，同时在名为name1、name2的
    //两个路由视图出口，渲染两个组件Foo、Bar
    components: {
      name1: Foo,
      name2: Bar,
    },
  },
  { path: "/foo", component: Foo },
  { path: "/bar", component: Bar },
];
```

> 命名路由视图

- 当要在同一路由下同时渲染多个组件，那么也要有多个路由出口<router-view>
- 此时需要告诉 Vue Router 哪个组件渲染到哪个出口(**路由出口-组件映射关系**)
- 像**具名插槽**一样，通过为各路由视图`<router-view>`命名--
  - 为其 name 属性赋值即可

```xml
<div id="app">
  <!-- 路由出口 -->
  <router-view name="name1"></router-view>
  <router-view name="name2"></router-view>
</div>
```

9. 使用 Watch 监听路由
   > **watch 属性(监听器)**

- 当数据变化，Vue 会将该数据的旧、新两个值作为参数传入其在 watch 中的对应的数据监听函数
- watch 属性除了监听数据，它还可以用于**监听路由**
- 应用场景：发生界面路由跳转后，通过**监听路由地址变化**来判断当前界面是从哪个路由跳转过来的
  > **How to do?**

new 出来的 VueRouter 对象中，提供`path`属性，值为字符串，保存当前路由，因此--

我们只需监听这个`path`属性即可--

```javascript
watch:{
  "$route.path": function( oldvalue, newvalue){
      //console.log(oldvalue, newvalue)
  }
}
```
