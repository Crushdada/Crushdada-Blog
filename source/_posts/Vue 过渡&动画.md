---
title: Vue 过渡&动画
tags: Vue
categories: Code
abbrlink: 20274
date: 2021-02-22 00:00:00
---


# Vue 过渡动画

# 类名过渡

Vue 在插入、更新或者移除 DOM 时，提供多种不同方式的应用过渡效果
<!-- more -->

如何给 Vue 控制的元素添加过渡动画--

1. 将需要执行动画的元素用`<transition>`组件包裹

- 它只会为第一个元素执行过渡动画
- 若想为多个元素，如 list 添加动画，使用`<transition-group>`
  - 渲染时，它会**默认**将包裹在内的多个元素置于`<span>`内
  - 通过设置它达到 tag 属性，指定要将其放入何种标签内。如：

```xml
<transition-group tag="ul"></transition-group>
```

1. 告诉 vue 在何时应用什么样的动画效果
   1. 通过为元素绑定不同状态时的类名
   2. vue 在进入/离开的过渡中，规定了 6 个 class 切换
   3. 详见：[过渡的类名](https://cn.vuejs.org/v2/guide/transitions.html#%E8%BF%87%E6%B8%A1%E7%9A%84%E7%B1%BB%E5%90%8D?fileGuid=hRT6wXQD6D3TxQhT)

如：

```css
.v-enter {
  //过渡前的状态
  opacity: 0; //opacity 为不透明度，此处为‘隐藏’
}
.v-enter-to {
  //过渡后的状态
  opacity: 1;
}
.v-enter-active {
  //过渡中的状态
  transition: all 3s; //此处设置<transition>内元素的所有属性过渡时间为3秒
}
```

CSS3 transition 属性设置过渡效果()，有四个属性，详见：[CSS3 transition 属性](https://www.runoob.com/cssref/css3-pr-transition.html?fileGuid=hRT6wXQD6D3TxQhT) 3. 设置`<transition>`标签的 name 属性，来区分不同元素的过渡动画，如：

```xml
<transition name="temp">
<p>Hello World</p>
</transition>
```

然后过渡类就可以这样写：将 v 替换为 name

```css
.temp-enter {
  opacity: 0;
}
```

4. 设置`<transition>`标签的 appear 属性，使**初始渲染页面时**展示过渡动画

```xml
<transition appear>
  <p>Hello World</p>
<transition/>
```

# JS 钩子过渡

5. 在`<transition>`属性中声明 JavaScript 钩子

```xml
<transition
  :before-enter="beforeEnter"
  :enter="enter"
  :after-enter="afterEnter"
></transition>
```

然后在模型中给出钩子函数即可：

```javascript
methods: {
  beforeEnter: function (el) {
    // el.style.opacity="0";
  },
  // 当与 CSS 结合使用时，回调函数 done 是可选的
  enter: function (el, done) {
    // ...
    done()
  },
```

## JS 钩子结合第三方库/CSS

- 这些钩子函数可以结合 CSS`transitions/animations`使用，也可以单独使用。
- 推荐对于仅使用 JavaScript 过渡的元素添加`:css="false"`，Vue 会跳过 CSS 的检测。这也可以避免过渡过程中 CSS 的影响。

## 当**仅用**JS 钩子过渡时

- 在`enter`和`leave`中必须使用`done`进行回调。否则，它们将被同步调用，过渡会立即完成。 而不执行 afterEnter()、afterLeave()
- 在`enter`和`leave`中必须使用`el.offsetWidth`或`el.offsetHeight`否则过渡无效
- 可使用第三方 JS 动画库-如 Velocity
- 若想使**初始渲染页面时**展示过渡动画 ，除了为`<transition>`加上 appear 属性，还需延时调用`done()`

```javascript
setTimeout(() => {
  done();
}, 0);
```

- 详见：[Vue 文档--过渡动画--JS 钩子](https://cn.vuejs.org/v2/guide/transitions.html#JavaScript-%E9%92%A9%E5%AD%90?fileGuid=hRT6wXQD6D3TxQhT)
- 也可自定义过渡动画的类名，详见[自定义过渡类名](https://cn.vuejs.org/v2/guide/transitions.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E8%BF%87%E6%B8%A1%E7%9A%84%E7%B1%BB%E5%90%8D?fileGuid=hRT6wXQD6D3TxQhT)

## 第三方动画库 Animated

    * 自定义类名+第三方库[Animate.css](https://animate.style/?fileGuid=hRT6wXQD6D3TxQhT)（需要多刷新几次）
    * 在Animated官方文档选好需要的动画样式
    * 导入animate库

```plain
<link rel="stylesheet" type="text/css" href="https://cdn.jsdelivr.net/npm/animate.css@3.5.1">
```

    * 以`animated 动画名`的形式赋值给自定义的类名即可，如下：

```xml
<transition
  enter-class=""
  enter-active-class="animated flash"
  enter--to-class=""
>
<transition/>
```

## 过渡模式

`<transition>`的默认行为：进入和离开同时发生，可通过设置该标签的 mode 属性来指定发生顺序--

    * `in-out`：新元素先进行过渡，完成之后当前元素过渡离开。
    * `out-in`：当前元素先进行过渡，完成之后新元素过渡进入。
