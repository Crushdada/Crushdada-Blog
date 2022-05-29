---
title: CSS两种盒模型

tags: CSS

categories: Code

abbrlink: 28417

date: 2021-04-08 00:00:00
---


盒模型：css 定义所有的 html 元素都可以拥有像盒子一样的外形和平面空间

## 标准盒模型(W3C 盒模型)

![图片](https://uploader.shimo.im/f/UxY5tqfSkdwb0WWe.png!thumbnail?fileGuid=QVhvKxHHRqhjgCpP)

元素实际宽度： width+左右 padding+左右 border+左右 margin；

元素实际高度： height+上下 padding+上下 border+上下 margin
<!-- more -->

## 怪异盒模型(IE 盒模型)

![图片](https://uploader.shimo.im/f/wrkRMbX2RzWfF9PI.png!thumbnail?fileGuid=QVhvKxHHRqhjgCpP)

元素实际宽度：width（content+padding+border）+margin

元素实际高度：height（content+padding+border）+margin

## 两种盒子模型的区别

- 使用标准盒模型，在 css 代码中设置的 width、height 就是 content 的宽和高
- 怪异盒设置的 width、height 是指（content+padding+border）的总和
  > **因此**

计算怪异盒模型元素实际占用页面大小只需要：设置的 width+2\*margin 即可

## CSS 指定元素的盒模型

为元素添加 CSS 样式：

| 值          | 描述                                                                                                                                                         |
| :---------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| content-box | 标准盒<br>宽度和高度分别应用到元素的内容框。<br>在宽度和高度之外绘制元素的内边距和边框。                                                                     |
| border-box  | 怪异盒<br>就是说，为元素指定的任何内边距和边框都将在已设定的宽度和高度内进行绘制。<br>通过从已设定的宽度和高度分别减去边框和内边距才能得到内容的宽度和高度。 |
| inherit     | 从父元素继承 box-sizing 属性的值。                                                                                                                           |
