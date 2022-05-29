---
title: PostMan：接口测试工具

tags: JavaScript

categories: Code

abbrlink: 28411

date: 2021-04-07 00:00:00
---


# 安装

百度自行下载安装，也有中文版的

> **它可以--**

# 模拟后端接口，返回给前端数据

## 模拟 mock 后端接口：

<!-- more -->

用来测试前端写好的接口

详见：[使用 Postman 的模拟（mock）后端服务](https://www.jianshu.com/p/f166fba66858?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation&fileGuid=vhCdVVwhC3HjpxHD)

## **原理：**

- mock-server：译为**后端服务器**
- postman 的这个 API 搭建了一个服务器，并提供了服务器 url

![图片](https://uploader.shimo.im/f/to4oeCs5DbrSZi0U.png!thumbnail?fileGuid=vhCdVVwhC3HjpxHD)

然后--

![图片](https://uploader.shimo.im/f/6VX8xdnLnrgoPpNl.png!thumbnail?fileGuid=vhCdVVwhC3HjpxHD)

你可以复制这个服务器的的 url

我这里生成的是：https://8a5386e8-7013-4639-954f-f7bd1fc8986f.mock.pstmn.io

![图片](https://uploader.shimo.im/f/TfqufMiD60iVTxSj.jpg!thumbnail?fileGuid=vhCdVVwhC3HjpxHD)

- 开发者在 postman 中创建 example，postman 就会在这个服务器上创建相应的接口文件

我这里指定的文件名为：**mockget**

- 因此我们可以在项目中(前端)向该接口发送请求，达到了测试前端接口的目的
- 通过访问**服务器 url/接口文件名**的格式

**如这样：**

![图片](https://uploader.shimo.im/f/Bf4j8jzFr4xvUBeL.png!thumbnail?fileGuid=vhCdVVwhC3HjpxHD)

## 值得一提的是：

- postman 创建的该接口服务器使用了 nginx 代理
- 通过**后端**在 Response 的请求头添加**Access-Control-Allow-Origin：\***，允许该资源谁都可以访问，以此解决资源的跨域权限问题。
  - 有时你需要设置地更细/更多配置才能解决跨域--

```json
access-control-allow-headers: Authorization, Content-Type, Depth, User-Agent, X-File-Size, X-Requested-With, X-Requested-By, If-Modified-Since, X-File-Name, X-File-Type, Cache-Control, Origin
access-control-allow-methods: GET, POST, OPTIONS, PUT, DELETE
access-control-allow-origin: *
access-control-expose-headers: Authorization
```
