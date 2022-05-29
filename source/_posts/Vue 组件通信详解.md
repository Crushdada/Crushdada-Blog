---
title: Vue 组件通信详解
tags: Vue
categories: Code
abbrlink: 15126
date: 2021-02-28 00:00:00
---


## 父子组件

父子组件即**组件嵌套**，例如：**实例**与`<template>`定义在其中的**局部组件**与就是一对父子组件。之前也说过**实例**就是一个组件，其中可以设置 methods、components 等对象，那么局部组件中也可设置 components，就是设置其**子组件，**如下 ↓

> **为自定义全局组件设置子组件**
<!-- more -->


```javascript
Vue.component("father", {
  template: "#father",
  components: {
    son: {
      template: "#son",
    },
  },
});
```

**注意：**子组件必须嵌套在父组件中使用(父组件的`<template>`中)

> **为自定义局部组件设置子组件**

```javascript
components: {
  "father": {
    template: "#father",
    components:{
      "son": {
        template: "#son"
      }
    }
  }
}
```

1. 父子组件通信
   > 父子组件**数据**传递

默认情况下，子组件无法访问父组件的**数据**。要传递数据递 ↓

在父组件`<template>`中使用 v-bind 传递(子组件标签上)：

```xml
<son v-bind:"自定义接收名称"="父组件数据"></son>
```

在子组件使用`props`接收：

```javascript
components:{
  "son": {
    template: "#id"
    props: ["自定义接收名称"]
  }
}
```

> 父子组件**方法**传递

同样，默认子组件是无法访问父组件的**方法**。若想要访问 ↓

1. 在父组件的`<template>`中，(使用子组件的地方)使用 v-on 传递：

```xml
<son v-on:"自定义接收名称"="父组件方法"></son>
```

2. 在子组件的`methods`中定义一个新方法：

```javascript
components:{
  "son": {
    template: "#id"
    methods:{
      new_func(){
        this.$emit("自定义接收名称");
      }
    }
  }
}
```

3. 然后通过方法`new_func()`就可以访问父组件的方法了
4. 或者不用定义一个新方法，直接在子组件的`<template>`中--

```xml
<button @click="$emit('自定义接收名称')">
      Click me to be welcomed
</button>
```

`$emit("自定义接收名称")`：触发当前实例上的事件，**所以步骤 2 中本质是在做联动--当自定义的方法被触发时，使用$emit 触发父组件的方法。**因此当然也可以直接在触发处直接使用$emit，不再添加新的方法作为中介

> **父组件访问子组件数据**

若父组件想访问子组件的数据？

子组件在访问父组件的方法时，通过**传递参数**实现传递数据给父组件--

只需要作为$emit()的第二个参数即可，如下 ↓

```javascript
$emit("自定义接收名称", 参数);
```

然后在父组件相应的方法处接收参数即可

> 命名规则

- **注册时**若使用驼峰命名法，如`"myComponent"`，那么使用时(标签名)要改用小写，加横线，如`"my-component"`
- **传递数据**时，同理，如 ↓

`parent-name="name"`，那么接收时：`props：["parentName"]`

- **传递方法**时，**无法使用驼峰命名**，可以用短横线分隔命名

2. 多级组件通信

在多级组件嵌套下，儿子组件若想访问爷爷组件的数据和方法，必须逐级传递
