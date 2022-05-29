---
title: Vue 选项/数据
tags: Vue
categories: Code
abbrlink: 46678
date: 2021-03-22 00:00:00
---


# Vue 选项/数据

## **computed 与 methods 的区别--**两个核心点，

1. **属性调用**(`methods`是方法调用)，直接用插值表达式`{{属性名}}`来调用，而 methods 需要在方法名后面加上"()"。
2. **能够缓存**，methods 中的函数，调用即执行，意味着有几处用到了该函数，就执行计算几次，而 computed 的话，只需调用计算一次，得到返回值即可，其他调用可直接使用该缓存的返回值，避免内存的浪费
3. 详见[https://cn.vuejs.org/v2/guide/computed.html](https://cn.vuejs.org/v2/guide/computed.html?fileGuid=xGdwgCTtQpKvgXPr)

---
<!-- more -->


## computed 计算属性

1. 用来**监控**自定义的变量，定义在与 data 同级的`computed`对象中，为双向数据绑定；
2. 形式为:

```xml
<p>{{计算属性名}}</p>
```

注意：在元素中使用计算属性时不能在后面加()，因为它是一个属性而不是函数

```javascript
computed:{
  计算属性名: function(){
  逻辑
  return res;
  }
}
```

1. 这样做比较适合对多个变量或者对象进行处理后返回一个结果值，也就是数多个变量中的某一个值发生了变化则我们监控的这个值也就会发生变化
2. 当你有一些数据需要随着其它数据变动而变动时，设置计算属性的 get 和 set 函数(这二者类似构造方法一样会自动调用)
3. get：用于 return 计算属性的新值
4. set：当程序侦听到计算属性被改变时，会传入其新的 value 值给 set()，允许通过这个 value 来改变 fullName 相关联的值，相应的页面上 fullName 也会更新。(侦听到值变了->传入新值->do something)
5. 详见[https://zhuanlan.zhihu.com/p/48641887](https://zhuanlan.zhihu.com/p/48641887?fileGuid=xGdwgCTtQpKvgXPr)

---

## **侦听器 watch**

### **What's is it?**

它是一个数据状态监听器，当监听到数据变化时，自动调用相应数据的 callback 函数

**注意：**

- 不同于计算属性 computed，watch 方法只能监听 data 中已存在的变量，否则报错。
- 不应该使用箭头函数来定义 watcher 函数
  _ 例如`searchQuery: newValue => this.updateAutocomplete(newValue))`
  _ 理由是**箭头函数绑定了父级作用域的上下文** \* 所以  this  将不会按照期望指向 Vue 实例，`this.updateAutocomplete`将是 undefined。
  > **当我们需要实现：**

由一次数据变化，引发其他的一些变化--

- 此时，我们往往需要关注**是什么导致了数据的变化**，然后在引发变化的地方**手动监听**，如监听键盘、鼠标的输入
- 而使用 Watch 属性，它只用关注**数据本身是否发生改变**，而不用我们关注引起变化的原因。
- 因此，它能够帮助我们**自动监听**

### How to do?

- `变量名: function (newValue, oldValue) {}`

或者

- `变量名(newValue, oldValue) {}`
- 它会传入新、旧两个值，以供操作。

详见[Vue API-Watch 属性](https://cn.vuejs.org/v2/api/#watch?fileGuid=xGdwgCTtQpKvgXPr)
