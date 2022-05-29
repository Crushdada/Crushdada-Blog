---
title: 应用视觉设计进阶-transform、animation和关键帧

tags: CSS

categories: Code

abbrlink: 28422

date: 2021-04-13 00:00:00
---



# Transform 属性

## Transform | scale：更改元素的大小

CSS 属性`transform`里面的`scale()`函数，可以用来改变**元素的显示比例**。

**举个雷子：**
<!-- more -->
实现斗鱼直播间窗口--悬停放大

参见--[CSS3 实现放大动效](https://www.zhihu.com/zvideo/1356294377577492480?fileGuid=CdtgkyXv8YTQqxy6)

## Transform | skex：沿 X 轴倾斜元素

- `skewX(x deg)`使元素沿着 X 轴（横向）翻转指定的角度。
- `skewY(x deg)`使元素沿着 X 轴（横向）翻转指定的角度。
- `rotate()`该用法和 skex 类似

# 使用 CSS 创建图形

> **术语表**

- blur-radius => 模糊半径，
- spread-radius => 辐射半径，
- transparent => 透明的，
- border-radius => 圆角边框

**举个雷子：**

```css
<style>
.heart {
  position: absolute;
  margin: auto;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: pink;
  height: 50px;
  width: 50px;
  transform: rotate(-45deg);
}
.heart:after {
  background-color: pink;
  content: "";
  border-radius: 50%;
  position: absolute;
  width: 50px;
  height: 50px;
  top: 0px;
  left: 25px;
}
.heart:before {
  content: '';
  background-color: pink;
  border-radius: 50%;
  position: absolute;
  width: 50px;
  height: 50px;
  top: -25px;
  left: 0px;
}
</style>
<div class = "heart"></div>
```

![图片](https://uploader.shimo.im/f/2oIQemQ3X8VDWNxz.png!thumbnail?fileGuid=CdtgkyXv8YTQqxy6)

---

# CSS 的关键帧和动画

> animation--动画

`animation`属性控制动画的外观，总共有 8 个`animation`属性。

`animation-name`设置动画的名称， 也就是要绑定的选择器的`@keyframes`的名称。

`animation-duration`设置动画所花费的时间。

> @keyframes--关键帧

`@keyframes`规则控制动画中各阶段的变化。

`@keyframes`能够创建动画。

- 创建动画的原理是将一套 CSS 样式逐渐变化为另一套样式。
- 具体是通过设置动画期间对应的“frames”的 CSS 的属性
  - 以百分比来规定改变的时间
  - 或者通过关键词“from”和“to”，等价于 0% 和 100%。
- 0% 是初始状态。100% 就是元素最后的样子

**举个雷子：**

动画开始时背景为蓝色，中间为绿色，最后为黄色

```css
#rect {
  animation-name: rainbow;
  animation-duration: 4s;
}
@keyframes rainbow {
  0% {
    background-color: blue;
  }
  50% {
    background-color: green;
  }
  100% {
    background-color: yellow;
  }
}
```

- 往往使用伪类，如:hover 搭配动画
- 但有一个问题：动画在设置的时间之后重置了，回到了动画前的样式
- 当有需求：而我们想要的效果是按钮在悬停时始终高亮
- 就需要将`animation-fill-mode`属性，来指定了**在动画结束时元素的样式**。
- 设置成`forwards`来实现。
- `animatio-fill-mode: forwards;`

## CSS 创建动画运动

- 当元素的`position`被指定，如`fixed`或者`relative`时
- 指定元素的偏移属性`right`、`left`、`top`和`bottom`可以用在动画规则里创建动作
- 结合关键帧@keyframes，能够实现元素的**动画运动**
- 试一试 ：[FreeCodeCamp--让元素动起来！](https://learn.freecodecamp.one/responsive-web-design/applied-visual-design/create-movement-using-css-animation?fileGuid=CdtgkyXv8YTQqxy6)

## 让动画一直动下去！

`animation-iteration-count`这个属性允许你**控制动画循环的次数**。

栗子：

`animation-iteration-count: 3;`

在这里动画会在运行 3 次后停止

如果想让动画**一直运行**，可以把值设置成**infinite（无限）**。

之前练习 CSS 动画和关键帧时写了一个爱心，现在，我们可以使用该属性让它跳动起来！

试一试：[FreeCodeCamp--CSS 写一个跳动的爱心！](https://learn.freecodecamp.one/responsive-web-design/applied-visual-design/make-a-css-heartbeat-using-an-infinite-animation-count?fileGuid=CdtgkyXv8YTQqxy6)

效果：

![图片](https://uploader.shimo.im/f/7G3PFZqYdWVrO02W.gif?fileGuid=CdtgkyXv8YTQqxy6)

## 为元素动画设置可变速率

- 可以通过**协调动画过渡时间**和**关键帧百分比**来改变动画的运动速率
- 也可直接设置`animation-duration`属性 \* 如：animation-duration: 1.1s;
  > 或者。。

## 使用关键字更改动画定时器

- `animation-timing-function`规定动画的速度曲线。
- 规定从样式 A 过渡到 样式 B 的过程中，运动中的加速和减速的过程。
- 已经有了很多**预定义的值**可以直接使用于大部分场景。
  - `ease`，慢--快--慢，它是默认值
  - `ease-out`，动画以高速开始，以低速结束;
  - `ease-in`，动画以低速开始，以高速结束；
  - `linear`，动画从头到尾的速度是相同的。

除了上述几个预定义值之外，CSS 还提供了--

> **贝塞尔曲线（Bezier curves）**

- 用`cubic-bezier`来定义贝塞尔曲线。
- **曲线的形状代表了动画的速度**。
- 曲线在 1\*1 的坐标系统内，曲线的 X 轴代表动画的时间间隔（类似于时间比例尺），Y 轴代表动画的改变。
- `cubic-bezier`函数包含了 1 \* 1 网格里的 4 个点：`p0`、`p1`、`p2`和`p3`。
- 其中`p0`和`p3`是固定值，代表曲线的起始点和结束点，坐标值依次为 (0, 0) 和 (1, 1)。
- 你只需设置另外两点的 x 值和 y 值，设置的这两点确定了曲线的形状从而确定了动画的速度曲线。
- 在 CSS 里面通过`(x1, y1, x2, y2)`来确定`p1`和`p2`。

**举个雷子：**

```css
animation-timing-function: cubic-bezier(0.25, 0.25, 0.75, 0.75);
```

- 在上面的例子里，两个点的 x 和 y 值相等（x1 = 0.25 = y1 和 x2 = 0.75 = y2），如果你还记得初中几何，结果是从原点到点 (1, 1) 的一条直线。动画速度呈线性，效果和`linear`一致。换言之，元素匀速运动。

**另一个栗子：**

模拟杂耍球运动

```css
animation-timing-function: cubic-bezier(0.311, 0.441, 0.444, 1.649);
```

效果：
![图片](https://uploader.shimo.im/f/ra9bLraJX9uFeqtA.gif?fileGuid=CdtgkyXv8YTQqxy6)
