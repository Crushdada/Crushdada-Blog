---
title: 将博客从GitHub迁移到Gitee
tags: Gitee Pages
categories: My_Blog
abbrlink: 23020
date: 2021-03-05 00:00:00
---

我的博客搬家啦~当你看到本篇时，意味着我已经将博客搬到了Gitee码云。

原因是GitHub Pages太不稳定，国内用户经常访问不到。

下面讲一讲如何将博客从GitHub迁移到Gitee--

<!--more-->

我先是Hexo+GitHub Pages搭建的博客，想搬家到码云，使用同一本地仓库即可。

只需要修改.config配置文件，将其中deploy一项设为GitHub和Gitee两个地址。

# 具体参考：

[Hexo+Github/Gitee搭建静态网站博客](https://blog.csdn.net/pilihaotian/article/details/103337959)

# 小坑

1. 最后用git bash部署时报错：Authentication failed for...(某https)

解决方法：

打开cmd面板，输入以下指令↓

```plain
git config --system --unset credential.helper
```
再重新部署就可以了
2. 部署时的另一个错误：Access deined: Repository Not Found，找不到要部署的仓库

于是我又检查了一遍.config文件，没错，确实是如下图的ssh地址

![图片](https://uploader.shimo.im/f/1qDS0T6ry2SrN4Ab.png!thumbnail?fileGuid=qGgHTjD8rgxCYTh6)

后来我发现原来不是配置文件的错，而是我码云仓库设置出了问题--

我的**仓库访问URL**和**仓库名称**不一致

而我在.config配置文件中写的ssh地址是码云根据**仓库名称生成的**，因此导致报错。

解决方法：

将下图圈起来的两处改一致即可

![图片](https://uploader.shimo.im/f/qS5dccpziKVewKJK.png!thumbnail?fileGuid=qGgHTjD8rgxCYTh6)

3. 实现以上全部步骤后，访问Crushdada.gitee.io，报错404 not Found

原因是我的Gitee Pages还没有更新

解决方法：

打开博客的仓库->服务->点击Gitee Pages->点击【更新】即可

![图片](https://uploader.shimo.im/f/5cGznUtx76VBA7Qj.png!thumbnail?fileGuid=qGgHTjD8rgxCYTh6)

之后每次更新博客内容后，也需手动更新Gitee Pages










