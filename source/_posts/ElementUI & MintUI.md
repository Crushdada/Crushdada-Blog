---
title: ElementUI & MintUI
tags: ElementUI
categories: Code
abbrlink: 12005
date: 2021-03-24 00:00:00
---


# ElementUI

一款基于 Vue 的桌面端 UI 框架，和`bootstrap`一样对原生 html 标签进行了封装，便于美化

## Let's start！

[ElementUI 官方文档](https://element.eleme.cn/2.0/#/zh-CN/component/slider?fileGuid=tXChcKXgdGhpVGqR)

<!-- more -->

在编译器中--安装该框架

```plain
npm i element-ui -S
```

在需要使用的 js 文件中

```javascript
import ElementUI from "element-ui"; //导入框架
import "element-ui/lib/theme-chalk/index.css"; //导入样式文件
Vue.use(ElementUI); //告诉Vue，我们要使用该插件
```

然后将样式代码 copy 到组件或实例中即可

## 按需导入

默认饿了么 UI 会导入整个库(所有封装的组件)到项目中，这样会导致项目体积大，用户访问慢

我们可以--借助 babel 插件，只会将我们用到的组件打包

### 修改配置

找到项目中的`babel.config.js`，修改为

```json
{
  "presets": [["es2015", { "modules": false }]],
  "plugins": [
    [
      "component",
      [
        {
          "libraryName": "element-ui",
          "styleLibraryName": "theme-chalk"
        }
      ]
    ]
  ]
}
```

### 导入用到的样式

```plain
import {button ,switch} from "element-ui";
Vue.use(button)  // 分别Use
Vue.use(switch)
```

# MintUI

另一个基于 Vue 的样式框架，也是出自饿了么

详见：[MintUI 文档](http://mint-ui.github.io/docs/#/zh-cn2/quickstart?fileGuid=tXChcKXgdGhpVGqR)

同样，它也支持按需引入

> **与饿了么 UI 的两点区别**

但与`ElementUI`步骤不同的是--

最后不使用`Vue.use`，作为替代--

```javascript
Vue.compoment(Button.name, Button);
Vue.compoment(Button.name, Switch);
```

第二个区别：
它还需要按需导入 css 文件，而饿了么 UI 按需导入时不用，直接导入样式组件即可

**不推荐在移动端开发中使用该框架，因为坑很多**
