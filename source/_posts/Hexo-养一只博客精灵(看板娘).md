---
title: Hexo-养一只博客精灵(看板娘)
tags:
  - Hexo
  - 看板娘
categories: My_Blog
abbrlink: 41358
date: 2021-02-13 00:00:00
---

博客搭建完毕之后，会不会觉得有些单调？那么，添加一只萌萌的看板娘吧！

<!--more-->

# 看板娘是什么样的？

像是这样↓

![图片](https://uploader.shimo.im/f/tGIQXKEZO1C95GYN.png!thumbnail?fileGuid=PYCrcrTwXrj6yPcY)

或者这样的↓

![图片](https://uploader.shimo.im/f/1ZHFLDWNXfbqU4r7.png!thumbnail?fileGuid=PYCrcrTwXrj6yPcY)

是不是很萌？

下面就展示如何操作！

![图片](https://uploader.shimo.im/f/jQiFr4v4ueCW7Hfi.jfif!thumbnail?fileGuid=PYCrcrTwXrj6yPcY)

# 需要工具

基于Hexo的搭建完毕的博客

hexo-helper-live2d插件

npm、git环境

# 安装hexo-helper-live2d 插件

```plain
npm install --save hexo-helper-live2d
```
# 下载live2d模型

下面给出看板娘模型的名单和预览图，你可以挑一个想要的

预览图：[https://huaji8.top/post/live2d-plugin-2.0/](https://huaji8.top/post/live2d-plugin-2.0/)

```plain
live2d-widget-model-chitose
live2d-widget-model-epsilon2_1
live2d-widget-model-gf
live2d-widget-model-haru/01 (use npm install --save live2d-widget-model-haru)
live2d-widget-model-haru/02 (use npm install --save live2d-widget-model-haru)
live2d-widget-model-haruto
live2d-widget-model-hibiki
live2d-widget-model-hijiki
live2d-widget-model-izumi
live2d-widget-model-koharu
live2d-widget-model-miku
live2d-widget-model-ni-j
live2d-widget-model-nico
live2d-widget-model-nietzsche
live2d-widget-model-nipsilon
live2d-widget-model-nito
live2d-widget-model-shizuku
live2d-widget-model-tororo
live2d-widget-model-tsumiki
live2d-widget-model-unitychan
live2d-widget-model-wanko
live2d-widget-model-z16
```
然后执行指令↓
```plain
npm install live2d-widget-model-koharu //假设你选择的看板娘是koharu
```
# 修改_config.yml配置文件

找到你博客的本地存储的根目录，打开_config.yml文件，添加下面的内容

```plain
#Live2D动画
live2d:
  enable: true
  scriptFrom: local
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/
  tagMode: false
  debug: false
  model:
    use: live2d-widget-model-koharu 
  display:
    position: right 
    width: 150
    height: 300
  mobile:
    show: true
```
# 主要参数说明

1. enable //是否使用
2. model:
use: live2d-widget-model-koharu  //要使用的模型名称
3. display:
position: right //显示的位置
width: 150 //宽度
height: 150 //高度
mobile:
show: true //移动端是否显示
# 调试&部署

在你已经安装git环境的基础上，在博客的本地存储根目录下右键->Git Bash Here

依次输入↓

```plain
hexo clean
hexo g
hexo s
```
现在你就可以在本地预览一下自己的博客精灵啦，打开浏览器登录：**localhost:4000**
或者，[点击这里](http://localhost:4000)~

相中好看板娘之后，就可以部署到GitHub pages上了

回到git，输入下面的指令并成功登录后...

```plain
hexo d
```
ohhhhhhhhh，Amazing！
# 更灵动的小家伙(能互动，可换装)

上面的模型较为简单，没有说话、互动等功能。或许，我们可以整点更牛的？

直接应用大神作品，功能齐全。能说话、能换装、能玩游戏、能拍照、还能自定义

像是这样↓

![图片](https://uploader.shimo.im/f/I86gQh2nVZ4CA6mm.png!thumbnail?fileGuid=PYCrcrTwXrj6yPcY)

转载自：[https://blog.csdn.net/u010519914/article/details/89440004](https://blog.csdn.net/u010519914/article/details/89440004)

进阶版实现步骤：[https://zhuanlan.zhihu.com/p/58325389](https://zhuanlan.zhihu.com/p/58325389)

# DIY你独有的看板娘！

再或者，你可以自定义创建独属于你的小精灵！

不过，这个过程可能要麻烦一点，除非你本就熟悉动画人物建模..、

建模工具可以是live2d软件，也可以是MMD--MiKuMiKuDance

至于如何实操，我是不会，不过网上已经有大量的教程了，加油！

# 一个值得期待的项目

就DIY看板娘这件事而言，我还在b站了解到一个值得期待的项目--[【AI老婆生成器】](https://github.com/pkhungurn/talking-head-anime-demo)，

只需要单张图片，一键生成 live2D 虚拟偶像。

该项目是一位在谷歌日本工作的软件工程师Pramook Khungurn业余时间开发的一个小软件，利用AI技术可以一键把静态二次元妹子图转换成VTuber~ 自带脸部捕捉功能。

详情见b站[https://www.bilibili.com/video/av83675725](https://www.bilibili.com/video/av83675725)

看了一下，虽然视频中该项目对图片进行建模的最终效果较为粗糙，但是结合[【waifu生成器】](https://waifulabs.com/)，一键生成妹子图之后再一键建模，十分的方便。而且随着时间的推移，这个项目功能会慢慢变得更加强大的，蛮值得期待呀。

