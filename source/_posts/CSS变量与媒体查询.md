---
title: CSS变量与媒体查询

tags: CSS3

categories: Code

abbrlink: 28418

date: 2021-04-09 00:00:00
---


# 使用 CSS 变量

## 创建：

- 在变量名前添加两个`破折号`并为其赋值，例子如下：

```css
--penguin-skin: gray;
```
<!-- more -->

- 这样会创建一个`--penguin-skin`变量，其值为`gray（灰色）`
- 可以通过**引用变量名**来使用它的值

```css
background: var(--penguin-skin);
```

## 给 CSS 变量附加回退值

可以设置一个**备用值**来防止由于某些原因导致变量不生效的情况

- 为了适配 CSS 变量的旧浏览器
- 或者，设备不支持你设置的变量值
- 为了防止这种情况出现，那么你可以这样写：

```css
background: var(--penguin-skin, black);
```

- 如此，当变量失效，浏览器会应用黑色

# 层级 CSS 变量

- CSS 变量，不仅可以在声明该变量的元素中使用，也可以在该元素的子元素中使用
- 这种效应称为 cascading（层叠）。
- 因为层叠的效果，CSS 变量通常会定义在`:root`元素里
- 定义在`:root`元素里的变量的**作用域**是整个页面

```css
:root {
  --penguin-belly: pink;
}
```

- 如果在元素里创建相同的变量，会**重写**`:root`变量设置的值。

# 媒体查询 media query

CSS 变量可以简化媒体查询的方式

例如，当屏幕小于或大于媒体查询所设置的值，通过改变变量的值，那么应用了变量的元素样式都会得到响应修改

```css
@media (max-width: 350px) {
  :root {
    /* add code below */
    --penguin-size: 200px;
    --penguin-skin: black;
    /* add code above */
  }
}
```
