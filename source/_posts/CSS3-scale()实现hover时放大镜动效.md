---
title: CSS3-scale()实现hover时放大镜动效
tags: CSS3
categories: Code
abbrlink: 37088
date: 2021-03-17 00:00:00
---


## CSS 设置图片 hover 时放大镜动效

    1. 需要给`<img>`加一个父元素，设置`overflow:hidden`
    2. 使用scale("比例")将元素基于中心按比例缩放
<!-- more -->

```css
 .father {
      cursor: pointer;
      width: 300px;
      height: 300px;
      overflow: hidden;
    }
    img {
      width: 100%;
      transition: transform 0.3s; /*规定设置过渡效果的CSS属性的名称和时间*/
    }
    img:hover {
      transform: scale(1.1);  /*放大1.1倍*/
    }
```

```xml
<div class="father">
      <img src="..."/>
</div>
```

CSS`transform`属性对元素应用 2D 或 3D 转换。该属性允许我们对元素进行旋转、缩放、移动或倾斜。详见：[CSS transform 属性](https://www.runoob.com/cssref/css3-pr-transform.html?fileGuid=jg8T3tjVGKJ6r8H8)
