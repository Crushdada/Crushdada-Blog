---
title: Vue自定义过滤器
tags: Vue
categories: Code
abbrlink: 53569
date: 2021-02-20 00:00:00
---


# 自定义全局过滤器

- 用于格式化文本内容
- 只能在插值处和 v-bind 处使用
- 可在一处元素堆叠使用多个过滤器

1. 使用`Vue.filter()`方法定义过滤器，该方法接收两个参数：过滤器名(String)，处理数据的函数
2. 默认处理数据的函数接收一个参数，即当前要被处理的数据
3. 使用时，在元素插值处，加上`| 过滤器名`即可，如下 ↓
<!-- more -->


```xml
<p>{{name | 过滤器1 | 过滤器2("参数")}}</p>
```

//name 的值为要处理的文本内容，作为默认参数 value 传入过滤器函数
//过滤器名后括号可传参,由于第一个参数默认是 value，则是从第二个参数开始接收

```plain
Vue.filter("过滤器名",function (value，parameter){
  业务逻辑
  return new_Value
})
```

4. 过滤器会将处理好的结果 return 到原来的位置

# 自定义局部过滤器

局部：作用域为**实例内**，不可用于其他 vue 实例

定义在实例中与 data 同级的**filters**对象内

```javascript
let vue1 = new Vue({
  el: "#app1",
  data: {},
  methods: {},
  filters: {
    过滤器名: function (value) {
      业务逻辑;
      return new_Value;
    },
  },
});
```
