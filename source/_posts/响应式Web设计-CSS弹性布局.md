---
title: 响应式Web设计-CSS弹性布局

tags: CSS

categories: Code

abbrlink: 28425

date: 2021-04-16 00:00:00
---


# css 弹性布局

## 认识弹性盒子

![图片](https://uploader.shimo.im/f/gI6nThy9vsG11Lh6.png!thumbnail?fileGuid=gxDXDvTwjTTDwHKy)

<!-- more -->

- 我们称--flexbox 为**弹性盒子**
- 为元素添加`display:flex`使其成为弹性盒子
- **它被一个主轴，一个交叉轴贯穿，二者默认指向分别为：从左到右，从上到下**
- **盒子中的子元素总是依据这两个轴排布**

## 弹性盒子(父元素)的属性

### flex-direction 属性：指定主轴方向

- `flex-direction`属性添加到父元素，可以控制主轴和纵轴的方向和起点
- 纵轴总会垂直于主轴
- 值为 row、column、row-reverse、column-reverse
  - row 让主轴水平方向穿过弹性盒子，从左-->右
  - column 让主轴在垂直方向穿过弹性盒子，从上-->下
  - 而给二者加上-reverse 能够让元素反过来排布。变成从右-->左，从下-->上

### justify-content 属性：指定子元素如何在主轴上对齐

- `justify-content`设置**flex 子元素在主轴上的对齐方式**
  - `justify-content:center;`：对于主轴居中
  - `flex-start`：从主轴起点开始排布
  - `flex-end`：从主轴终点开始排布
  - `space-between`：第一个和最后一个项目会**紧贴到容器边沿，**其他项目保留**一定间距**地在主轴均匀排布。
  - `space-around`：与`space-between`相似，但头尾两个项目不会紧贴容器边缘，其他项目也是均匀排布

### align-items 属性：指定子元素如何在交叉轴上对齐

flex-start/flex-end/center/这几个和主轴上对齐方式一个意思

**需要注意的是：**

- stretch(默认值)：如果项目未设置高度或设为 auto，将占满整个容器的高度

![图片](https://uploader.shimo.im/f/sbUgodUkbHKy6a37.png!thumbnail?fileGuid=gxDXDvTwjTTDwHKy)

- baseline: 项目的第一行文字的基线对齐

![图片](https://uploader.shimo.im/f/e03dCrHlWl9AT3VS.png!thumbnail?fileGuid=gxDXDvTwjTTDwHKy)

### align-content 属性：指定子元素如何在主轴上对齐

与 justify-content 用法相同

### flex-wrap 属性：换行

项目都排在一条线（又称"轴线"）上，flex-wrap 属性定义，如果一条轴线排不下，如何换行：

```css
.box {
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```

### flex-flow 属性：主轴方向+换行的简写

flex-flow 属性是 flex-direction 属性和 flex-wrap 属性的**简写形式**，默认值为 row nowrap

```css
.box {
  flex-flow: <flex-direction> || <flex-wrap>;
}
```

详见：[Flex 弹性盒子布局详解--潘亚](https://www.cnblogs.com/jpwz/p/12483053.html?fileGuid=gxDXDvTwjTTDwHKy)

## 盒子中子项目(子元素)的属性

### order 属性

order 属性定义项目的排列顺序，数值越小，排列越靠前，默认为 0：

```css
.item {
  order: integer; //整数
}
```

![图片](https://uploader.shimo.im/f/aw179gBVCULRpalM.png!thumbnail?fileGuid=gxDXDvTwjTTDwHKy)

### flex-grow 属性：放大

- flex-grow 属性定义项目的放大比例，默认为 0，即如果存在剩余空间，也不放大
- 如果所有项目的 flex-grow 属性都为 1，则它们将等分剩余空间（如果有的话）
- 如果一个项目的 flex-grow 属性为 2，其他项目都为 1，则前者占据的剩余空间将比其他项多一倍

```plain
.item {
    flex-grow: <number>; /* default 0 */
}
```

![图片](https://uploader.shimo.im/f/oviSb2a55FT7I3h5.png!thumbnail?fileGuid=gxDXDvTwjTTDwHKy)

- 如果有的项目有 flex-grow 属性，有的项目有 width 属性，
- 有 flex-grow 属性的项目将等分剩余空间

![图片](https://uploader.shimo.im/f/wsskgx7nAgXAMCyS.png!thumbnail?fileGuid=gxDXDvTwjTTDwHKy)

### flex-shrink 属性：缩小

- 定义了项目的缩小比例，**默认为 1，即如果空间不足，该项目将缩小**
- 如果所有项目的 flex-shrink 属性都为 1，当空间不足时，都将等比例缩小
- 如果一个项目的 flex-shrink 属性为 0，其他项目都为 1，则空间不足时，前者不缩小

```css
.item {
  flex-shrink: <number>; /* default 1 */
}
```

### align-self 属性：特立独行者

**允许单个项目有与其他项目不一样的对齐方式，可覆盖 align-items 属性**

默认值为 auto，表示继承父元素的 align-items 属性，如果没有父元素，则等同于 stretch

```css
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

### flex-basis 属性

指定了项目在 CSS 进行`flex-shrink`或`flex-grow`调整前的初始大小。

`flex-basis`属性的单位与其他 size 属性一致（`px`、`em`、`%`等）。如果值为`auto`，项目的大小依赖于自身内容。

**举个栗子：**

给`#box-1`和`#box-2`添加 CSS 属性`flex-basis`。分别设置为`10em`，`20em`。

```css
#box-container {
  display: flex;
  height: 500px;
}
#box-1 {
  background-color: dodgerblue;
  height: 200px;
  flex-basis: 10em;
}
#box-2 {
  background-color: orangered;
  height: 200px;
  flex-basis: 20em;
}
```

那么它们的效果如下：
![图片](https://uploader.shimo.im/f/XaMPfID7J2xw9ywV.png!thumbnail?fileGuid=gxDXDvTwjTTDwHKy)

# 关于 li 标签是否换行的问题：

- 默认自动换行，即一个 li 标签独占一行，但是有时会**从父元素继承左浮动**（float:left），此时就不会独占一行
  > **要使每个 li 独占一行**

1. 可以对 li 标签使用 clear:both 或者 clear:left 清除一下。
   - 要注意精准一点，以免污染其他需要左浮动的 li 标签
2. 可以直接给 li 标签 100%的**width**

行内元素不可以设置宽高，但是可以设置     左右 padding、左右 margin

3. 可以将包裹它的 ul 或 ol(块状元素)设置 white-space:nowrap
   1. 然后设置 li 为行内块

```css
.footer ul  {
    white-space: nowrap;
    width: fit-content;
    height: 2em;
}
.footer li  {
    display: inline-block;
}
```
