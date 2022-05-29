---
title: Hexo+GitHub-page=My_Blog!
tags:
  - Hexo
  - GitHub Pages
categories: My_Blog
abbrlink: 42145
date: 2021-02-09 00:00:00
---

## 整体步骤参考链接

[https://jingyan.baidu.com/article/d8072ac47aca0fec95cefd2d.html](https://jingyan.baidu.com/article/d8072ac47aca0fec95cefd2d.html)

<!--more-->
## 两个主题

* landscap+：[https://github.com/xiangming/landscape-plus.git](https://github.com/xiangming/landscape-plus.git)

![图片](https://uploader.shimo.im/f/bBTovn3MT02oY1ke.png!thumbnail?fileGuid=TRqgCkwjwX3gKw6g)

* next：[https://github.com/theme-next/hexo-theme-next.git](https://github.com/theme-next/hexo-theme-next.git)themes/hexo-theme-next

![图片](https://uploader.shimo.im/f/8KpiKtwnQ6VomQI3.png!thumbnail?fileGuid=TRqgCkwjwX3gKw6g)


当然，上面两幅图只是原始框架，后续可以进行一系列DIY

* 比如这样↓

![图片](https://uploader.shimo.im/f/pmKaPM2xHZ4ULF1B.png!thumbnail?fileGuid=TRqgCkwjwX3gKw6g)

* 然后这样↓

![图片](https://uploader.shimo.im/f/rn7JZbh4xLuvYjTI.png!thumbnail?fileGuid=TRqgCkwjwX3gKw6g)

* You can do whatever you want here!
## 自定义Blog各部分的样式

1. 更改背景、字体颜色：
    1. 找到你的blog本地存储位置\themes\hexo-theme-next\source\css\_variables\Mist.styl
    2. (这里我所使用的副主题名字是Mist，你的可以在你的博客页脚看到)![图片](https://uploader.shimo.im/f/8l2jXIFN2cJ2zegQ.png!thumbnail?fileGuid=TRqgCkwjwX3gKw6g)
    3. 该样式文件中都是语义化的属性，直接改等号右边的颜色值就行，如$content-bg-color = rgba(0,0,0,0.5);
    4. 找到--你的blog本地存储位置\themes\hexo-theme-next\source\css\_colors.styl
    5. 从中修改相应部分的颜色即可
2. 为body部分添加背景图片
    1. 找到--你的blog本地存储位置\source
    2. 新建**名为"_data"**的文件夹
    3. 在**_data**文件夹下新建styles.styl文件
    4. 输入以下代码
```css
body {
 	background:url(/images/background.jpg);
 	background-repeat: no-repeat;
	background-size:cover;
    background-attachment:fixed;
    background-position:50% 50%;
}
```
    5. 然后将你想要的背景图片命名为background.jpg
    6. 将其存储到--你的blog本地存储位置\themes\hexo-theme-next\source\images下即可
### 3.设置侧边栏一行显示多个友链

[转载：https://blog.csdn.net/weixin_43971764/article/details/96583122](https://blog.csdn.net/weixin_43971764/article/details/96583122)

* NEXT主题官网：[http://theme-next.iissnan.com/theme-settings.html](http://theme-next.iissnan.com/theme-settings.html)
## 如何编辑并发布文章

1. 创建一篇文章
```plain
hexo new "文章名字"
```
创建的文章会出现在 source/_posts 文件夹下，是 MarkDown 格式。
可以使用记事本直接编辑

2. 填写文章首部信息
```plain
---
title: demo # 自动创建
date: 2020-03-03 17:01:24 # 自动创建
tags:
- Github
categories:
- Github
---
## 标题1
正文1
## 标题2
正文2
```
开头下方撰写正文，MarkDown 格式书写即可。
3. 使用编辑器进行编辑
    1. 可以简单一些
    2. 直接使用MarkDown或石墨文档编辑内容
        1. 必须在文章头部加上上述**首部信息**(必须是文本格式)
        2. 你可以为一篇博文设置多个tags
```plain
tags: [标签1,标签2,...标签n]
```
    3. 导出为MarkDown格式，替换原来的文件即可
* 这样在下次编译的时候就会自动识别标题、时间、标签、类别等等，另外还有其他的一些参数设置，可以参考文档：[https://](https://link.zhihu.com/?target=https%3A//hexo.io/zh-cn/docs/writing.html)[hexo.io/zh-cn/docs/writ](https://link.zhihu.com/?target=https%3A//hexo.io/zh-cn/docs/writing.html)[ing.html](https://link.zhihu.com/?target=https%3A//hexo.io/zh-cn/docs/writing.html)。
## 踩坑

1. 注意分支

确保你选择要部署到的分支是您想要的分支

    1. 确保你的博客本地存储位置中的_config.yml文件中branch的值为**默认分支名**

![图片](https://uploader.shimo.im/f/jt4ZhSPBpaCKvnt4.png!thumbnail?fileGuid=TRqgCkwjwX3gKw6g)

    2. 假设你只有main和master两个分支，那么从 2020 年 10 月 1 日开始之后GitHub默认分支已经由master改为main，我这里选择部署到master分支，我就需要先修改默认分支为master。
    3. 关于如何修改默认分支：

[https://blog.csdn.net/xuchaoxin1375/article/details/111414527](https://blog.csdn.net/xuchaoxin1375/article/details/111414527)

关于为何Github要修改主库名字：

[https://blog.csdn.net/cunfuxiao7305/article/details/109050545](https://blog.csdn.net/cunfuxiao7305/article/details/109050545)

![图片](https://uploader.shimo.im/f/UnBSkZkgl0NN9VuT.png!thumbnail?fileGuid=TRqgCkwjwX3gKw6g)

2. Hexo渲染工具被删除
* 我的Blog搭建成功之后，过了几天再次登录，竟然发现无了，报错如下

![图片](https://uploader.shimo.im/f/h1k2lh5gXEd8Tc0Q.png!thumbnail?fileGuid=TRqgCkwjwX3gKw6g)原因是：hexo5.0之后，将hexo渲染工具删除了，需**重新手动安装**，执行如下命令：

```plain
npm install hexo-renderer-pug hexo-renderer-stylus --save
```
之后重新部署到GitHub即可
3. 你需要刷新
* 部署完成后，刷新你的博客页面可能不会立即看到效果，你需要刷你的Github页面或者等一等
4. 属性与值之间的空格
* 切记--设置config.yml文件，每个属性后面的冒号和值之间都要有一个英文空格，否则无法生效
5. hexo d部署失败

报错：fatal: unable to access 'https://xxxx/': OpenSSL SSL_read: Connection was reset, errno 10054

解决方法：

```plain
git config http.sslVerify “false”
```
若报错：fatal: not in a git directory，继续执行↓
```plain
git config --global http.sslVerify “false”
```
转载自：[https://blog.csdn.net/qq_44209563/article/details/104294486](https://blog.csdn.net/qq_44209563/article/details/104294486)
### 5.css样式失效了...吗？

* localhost运行良好，然后部署完登录页面发现css样式罢工
* 结果等了快5分钟才加载出来，原来是拖延症呀...
* css样式Firefox、chrome和Microsoft Edge都兼容完好
* 尝试优化图片加载
## 未完待续ing...



