---
title: Vue2核心原理

tags: Vue

categories: Code

abbrlink: 28396

date: 2021-04-04 00:00:00
---


#

# 数据双向绑定原理(数据劫持原理)

- 深度遍历实例 data 中的所有属性，
- 通过`Object.defineProperty()`方法将它们全部变成`getter`、`setter`以此进行**数据劫持**
- 每个组件实例都对应一个 watcher 实例，它会在组件渲染的过程中把“接触”过的**数据 property 记录为依赖**。之后当**依赖项的 setter 触发时**，会**通知 watcher**，从而使它关联的组件**重新渲染**
<!-- more -->

![图片](https://uploader.shimo.im/f/x4CIElOFA8z6r6HZ.png!thumbnail?fileGuid=ktPhgvdpwP3kgtGd)

**注意：**

- Vue 会在初始化实例时实现 data=>setter 和 getter 的转化，因此只有 data 中的数据才会是响应式的
- `Object.defineProperty()`方法是是 ES5 中一个无法**shim**的特性，这也就是 Vue**不支持 IE8 以及更低版本**浏览器的原因。

## `Object.defineProperty()`

在一个对象上定义新属性，或者修改对象的现有属性并返回这个对象

> [参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty#%E5%8F%82%E6%95%B0?fileGuid=ktPhgvdpwP3kgtGd)

`obj`

**要定义属性的对象。**

`prop`

**要定义或修改的属性的名称或**`Symbol`**。**

`descriptor`

**要定义或修改的属性描述符。**

> [返回值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty#%E8%BF%94%E5%9B%9E%E5%80%BC?fileGuid=ktPhgvdpwP3kgtGd)

被传递给函数的对象。

## `set`和`get`方法

```javascript
const object1 = {};
Object.defineProperty(object1, "property1", {
  get() {
    return val;
  },
  set(newval) {
    val = newval;
  },
});
object1.property1 = 77;
// throws an error in strict mode
console.log(object1.property1);
//77
```

set 和 get 是其内置方法，可直接设置 writable 属性为 true，来允许改写对象属性

```javascript
const object1 = {};
Object.defineProperty(object1, "property1", {
  writable: true,
});
object1.property1 = 77;
// throws an error in strict mode
console.log(object1.property1);
```

# [数组更新检测](https://cn.vuejs.org/v2/guide/list.html#%E6%95%B0%E7%BB%84%E6%9B%B4%E6%96%B0%E6%A3%80%E6%B5%8B?fileGuid=ktPhgvdpwP3kgtGd)

对于 data 中的数组中的属性不是响应式的，它们没有被监听，因此无法通过直接赋值触发重绘

```plain
vm.arr[0] = 'newName'; //不是响应性的
```

# Vue.set() API

### [Vue.set( target, propertyName/index, value )](https://cn.vuejs.org/v2/api/#Vue-set?fileGuid=ktPhgvdpwP3kgtGd)

- 参数：
  - `{Object | Array} target`
  - `{string | number} propertyName/index`
  - `{any} value`

用法：

向响应式对象中添加一个 property，并确保这个新 property 同样是响应式的，且触发视图更新。它必须用于向响应式对象上添加新 property，因为 Vue 无法探测普通的新增 property (比如`this.myObject.newProperty = 'hi'`)

- 注意对象不能是 Vue 实例，或者 Vue 实例的根数据对象。

### [变更方法](https://cn.vuejs.org/v2/guide/list.html#%E5%8F%98%E6%9B%B4%E6%96%B9%E6%B3%95?fileGuid=ktPhgvdpwP3kgtGd)

Vue 将被侦听的数组的变更方法进行了包裹，所以它们也将会触发视图更新。这些被包裹过的方法包括：

- ` push()``向数组的末尾添加一个或多个元素，并返回新的长度 `
- ` shift()``数组中删除第一个元素，并返回该元素的值 `
- ` pop()``删除最后一个元素，并返回该元素的值 `
- ` unshift()``将一个或多个元素添加到数组的开头，并返回该数组的新长度 `
- `splice()`**删除**或**替换**现有元素或者原地**添加新的元素**来修改数组,并以数组形式返回被修改的内容
- `sort()`
- `reverse()`

你可以打开控制台，然后对前面例子的`items`数组尝试调用变更方法。比如`example1.items.push({ message: 'Baz' })`。

### [替换数组](https://cn.vuejs.org/v2/guide/list.html#%E6%9B%BF%E6%8D%A2%E6%95%B0%E7%BB%84?fileGuid=ktPhgvdpwP3kgtGd)

变更方法，顾名思义，会变更调用了这些方法的原始数组。相比之下，也有非变更方法，例如`filter()`、`concat()`和`slice()`。它们不会变更原始数组，而总是返回一个新数组。当使用非变更方法时，可以用新数组替换旧数组：

```plain
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```

你可能认为这将导致 Vue 丢弃现有 DOM 并重新渲染整个列表。幸运的是，事实并非如此。Vue 为了使得 DOM 元素得到最大范围的重用而实现了一些智能的启发式方法，所以用一个含有相同元素的数组去替换原来的数组是非常高效的操作。
