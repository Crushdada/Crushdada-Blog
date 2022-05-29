---
title: Web可访问性/应用无障碍

tags: HTML5

categories: Code

abbrlink: 28423

date: 2021-04-14 00:00:00
---


# Web 可访问性/应用无障碍

## H5 新标签--标签语义化

HTML5 添加了诸如`main`、`header`、`footer`、`nav`、`article`、`section`等新标签，它们为开发人员提供更多的选择和**辅助特性**。(能够辅助**屏幕阅读器**工作)
<!-- more -->

- `section、div、article`的区别：
- `article`用于独立的、完整的内容
- `section`用于对与主题相关的内容进行分组。它们可以根据需要嵌套着使用。
- 举个例子：如果一本书是一个`article`的话，那么每个章节就是`section`。当内容组之间没有联系时，可以使用`div`。
  > <div> - 内容组
  > <section> - 有联系的内容组
  > <article> - 独立完整的内容
- `nav`：主导航链接

## <audio>：音频控件

- **使用它：用该标签将音频源<source>包裹**
- HTML5 的`audio`标签用于呈现音频内容，可以在`audio`上下文中为音频内容添加**文字说明**或者**字幕链接**，使听觉障碍用户也能获取音频中的信息。
- `audio`的`controls`属性：使浏览器为音频提供具有**开始、暂停等**功能的**播放控件**。
  - `controls`是一个布尔属性，只要这个属性出现在`audio`标签中，浏览器就会开启播放控件。

举个例子：

> <audio controls>
> <source src="audio/meow.mp3" type="audio/mpeg" />
> <source src="audio/meow.ogg" type="audio/ogg" />
> </audio>

## 使用 figure 元素提高图表的可访问性

- HTML5 引入了`figure`标签以及与之相关的`figcaption`标签。
- 它们一起用于展示可视化信息（如：图片、图表）及其标题。
- 这样通过语义化对内容进行分组并配以用于解释`figure`的文字，可以极大的提升内容的可访问性。

对于图表之类的可视化数据，标题可以为屏幕阅读器用户提供简要的说明。但是这里有一个难点，如何处理那些超出屏幕可视范围（使用 CSS ）的表格版本的图表数据，以使屏幕阅读器用户也可以获取信息。

举个例子——注意`figcaption`包含在`figure`标签中，并且可以与其他标签组合使用：

> <figure>
> <img src="roundhouseDestruction.jpeg" alt="Photo of Camper Cat executing a roundhouse kick">
> <br>
> <figcaption>
> Master Camper Cat demonstrates proper form of a roundhouse kick.
> </figcaption>
> </figure>

## 语义化呈现单选按钮组

使用`<fieldset>`包裹单选按钮组

还可使用`<legend>`来为单选按钮组**提供文字说明**。(屏幕阅读器会读取该标签中文字)

**举个栗子：**

```xml
<form>
  <fieldset>
    <legend>What level ninja are you?</legend>
    <input id="newbie" type="radio" name="levels" value="newbie">
    <label for="newbie">Newbie Kitten</label><br>
    <input id="intermediate" type="radio" name="levels" value="intermediate">
    <label for="intermediate">Developing Student</label><br>
    <input id="master" type="radio" name="levels" value="master">
    <label for="master">Master</label>
  </fieldset>
</form>
```

![图片](https://uploader.shimo.im/f/BTT7JaFhK3SW2PqG.png!thumbnail?fileGuid=h6cdCpxr6XHWdxvk)

## <input>： 日期选择器

- `input`标签的`type`属性指定了标签类，如`text`与`submit`类型的`input`标签
- HTML5 引入了`date`类型来创建时间选择器。以供用户通过点击来填写日期
- 当点击`input`标签时，依赖于浏览器的支持，**时间选择器会显示出来**，这可以让用户填写表单更加容易。
- 对于**旧版本**的浏览器，`type`属性的默认值是`text`。这种情况下，可以利用`label`标签或者占位文本来提示用户`input`标签的输入类型为日期。
  - 举个栗子：

```xml
<input type="date" id="pickdate" name="date">
```

![图片](https://uploader.shimo.im/f/AybYV2srnO18RjlS.png!thumbnail?fileGuid=h6cdCpxr6XHWdxvk)

点击它。。

![图片](https://uploader.shimo.im/f/QEiPtFkK01HjuDFF.png!thumbnail?fileGuid=h6cdCpxr6XHWdxvk)

## H5 的 datatime 属性：标准化时间

- HTML5 还引入了`time`标签与`datetime`属性来标准化时间。
- `<time>`是一个行内标签，用于在页面中呈现日期或时间，而`datetime`属性保存了日期的有效格式，**辅助设备可以访问这个值**。
- 通过标准化时间格式，即使时间在文本中是以非正式的或口语化的形式编写，辅助设备依然可以获取准确的时间和日期。

举个例子：

`<p>Master Camper Cat officiated the cage match between Goro and Scorpion <time datetime="2013-02-13">last Wednesday</time>, which ended in a draw.</p>`

## 自定义 CSS 让元素仅对屏幕阅读器可见

当信息以可视化形式（如：图表）展示，而屏幕阅读器用户需要一种替代方式（如：表格）来获取信息时，该如何做呢？

CSS 被用来**将**这些**仅供屏幕阅读器使用的信息，定位到浏览器可见区域之外**。

举个雷子：

```css
.sr-only {
    position: absolute;
    left: -10000px;
    width: 1px;
    height: 1px;
    top: auto;
    overflow: hidden;
}
```

**注意：**
下面的 CSS 代码与上面的结果不同：

- `display: none;`或`visibility: hidden;`会把内容彻底隐藏起来，对于屏幕阅读器也不例外
- 如果把值设置为 0px，如`width: 0px; height: 0px;`，意味着让元素脱离文档流，这样做也会让元素被屏幕阅读器忽略。

## accesskey 属性：指定快捷键

- HTML 提供 accesskey 属性，用于指定激活标签或者使标签获得焦点的快捷键，这可以使键盘用户在链接之间快速导航
- HTML5 允许在任何标签上使用这个属性。该属性对于交互类标签（如链接、按钮、表单控件等）十分有用。”

举个例子：

`<button accesskey="b">Important Button</button`

## 使用 tabindex 将键盘焦点添加到元素中

当用户在页面中使用 tab 键时，有些标签，如：链接、表单控件，可以自动获得焦点。它们获得焦点的顺序与它们出现在文档流中的顺序一致。

我们可以通过将`tabindex`属性值设为 0，来给其他标签赋予相同的功能，如：`div`、`span`、`p`等。举个例子：

`<div tabindex="0">I need keyboard focus!</div>`

**注意：**

`tabindex`属性值为负整数（通常为 -1）的标签也是有焦点的，只是不可以通过 tab 键来获得焦点。这种方法通常用于以编程的方式使内容获得焦点（如：激活用于弹出框的`div`标签）

使用`tabindex`属性可以使 CSS 伪类`:focus`在标签上生效
