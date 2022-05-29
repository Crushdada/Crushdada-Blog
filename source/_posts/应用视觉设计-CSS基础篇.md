---
title: 应用视觉设计-CSS基础篇

tags: CSS

categories: Code

abbrlink: 28419

date: 2021-04-10 00:00:00
---



# 应用视觉设计

## 文本样式

### text-align：文本对齐

### 使用 text-align 属性创建视觉平衡

`text-align`属性：控制**文本的对齐方式\*\*\*\***

- `text-align: justify;`两端对齐，即每行的左右两端都紧贴行的边缘。
- `text-align: center;`文本**居中对齐**。
- `text-align: right;`文本**右对齐**。
- `text-align: left;`是的默认值，文本**左对齐**。

### <sup>和<sub>：文本上下标
<!-- more -->
- 包裹在<sup></sup> 中的内容将会以**当前文本流**中**字符高度的一半**来**显示**，但是与当前文本流中文字的字体和字号都是一样的
- <sub>用来写下标

![图片](https://uploader.shimo.im/f/ShNuB45LAhzJWcSd.png!thumbnail?fileGuid=Xr6VYjPWJdvYKGc9)这是上标

### <strong> ：文本加粗

- 使用<strong>标签包裹需要加粗的**文本**
- 添加后，浏览器会自动给元素应用`font-weight:bold;`

```css
 <p>Google 由在<strong>斯坦福大学</strong>攻读理工博士的拉里·佩奇和谢尔盖·布林共同创建。</p>
```

### u 文本下划线

- 使用`<u>`标签来给文字添加下划线
- 添加后，浏览器会自动给元素应用`text-decoration: underline;`

### <em> ：强调文本

- emphasis => em => 强调
- 浏览器会自动给元素应用`font-style: italic`，所以文本会显示为斜体。

**其他突出文本的方法：**

    * **Jquery的highlight，能够改变背景颜色**
    * **<mark>：H5新标签，给文本加记号，使文本高亮**
    * **blink：JS方法，显示闪动的字符串，无法用于Chrome、ie、safari**

### <s>：添加删除线

- Strikethrough => s => 划线
- 添加后，浏览器会自动给元素应用`text-decoration: line-through;`。

### < hr> ：创建分割线

- Horizontal Rule => hr => 水平线
- 创建一条宽度撑满父元素的水平线
- 它一般用来表示文档主题的改变，在视觉上将文档分隔成几个部分。

![图片](https://uploader.shimo.im/f/b9yIh8iSCYw3F76U.png!thumbnail?fileGuid=Xr6VYjPWJdvYKGc9)

## box-shadow：阴影

`box-shadow`给元素**添加阴影**，该属性值是由逗号分隔的一个或多个阴影列表。

`box-shadow`属性的每个阴影依次由下面这些值描述：

- `offset-x`阴影的水平偏移量； offset：偏移量
- `offset-y`阴影的垂直偏移量;
- `blur-radius`模糊距离； blur：使模糊
- `spread-radius`阴影尺寸；
- 颜色。

其中`blur-raduis`和`spread-raduis`是可选的。

```css
box-shadow: 0 10px 20px rgba(0, 0, 0, 0.19), 0 6px 6px rgba(0, 0, 0, 0.23);
```

## opactiy：改变元素的透明度

CSS 里的`opacity`属性用来设置元素的透明度。

> 值 1 代表完全不透明。
> 值 0.5 代表半透明。
> 值 0 代表完全透明。

- 透明度会应用到元素内的**所有内容**，不论是图片，还是文本，或是背景色

## text-transform：改变文本大小写

`text-transform`属性来改变英文中字母的大小写。它通常用来统一页面里英文的显示，且无需直接改变 HTML 元素中的文本。

下面的表格展示了`text-transform`的不同值对文字 “Transform me” 的影响。

| Value      | Result                           |
| :--------- | :------------------------------- |
| lowercase  | "transform me"                   |
| uppercase  | "TRANSFORM ME"                   |
| capitalize | "Transform Me"                   |
| initial    | 使用默认值                       |
| inherit    | 使用父元素的 text-transform 值。 |
| none       | **Default:**不改变文字。         |

## font-weight：设置字体的粗细

注意：该属性的值的单位不是 px

```css
font-weight: 800;
```

## line-height：设置行间距/行高

行高，顾名思义，用来设置每行文字所占据的垂直空间。

## 伪类

- 伪类是可以添加到选择器上的关键字，用来选择元素的指定状态。
- 比如，`:hover`伪类选择器

### :hover--调整锚点的悬停状态

### :before--设置选择元素之前添加一些内容

### :after--设置选择元素之后添加一些内容

`:before`和`:after`必须配合`content`来使用。

这个属性通常用来给元素添加内容诸如图片或者文字。

当`:before`和`:after`伪类用来添加某些形状而不是图片或文字时，`content`属性仍然是必需的，此时可以设置它为空字符串。
