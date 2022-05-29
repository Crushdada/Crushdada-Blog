---
title: csrf攻击
tags: 面经
categories: Code
abbrlink: 32264
date: 2021-06-03 00:00:00
---


# What

CSRF（Cross-site requestforgery）--**跨站请求伪造**，

缩写为：CSRF/XSRF。
<!-- more -->

# 它会做什么

攻击者会欺骗浏览器，在**你已经登录、认证过的一些网站中盗用你的身份，以你的名义发送恶意请求**

CSRF 能够做的事情包括：以你名义**发送邮件，发消息，盗号**，甚至于用你的身份购买商品等

# 防范？

> > CSRF 的两个特点：

- CSRF（通常）发生在第三方域名
- 由于浏览器的同源策略，攻击者只能冒用你的身份发送他指定的请求，而**不能**直接**获取到 Cookie**中的认证**信息**。

## 方法概述：

我们可以针对以上两点进行防范：

- 阻止不明外域的访问
  - 同源检测
  - Samesite Cookie
- 提交时要求附加本域才能获取的信息
  - CSRF Token
  - 双重 Cookie 验证

由于 CSRF 的**本质在于攻击者欺骗用户去访问自己设置的地址**，所以--如果在请求时，要求客户端提供不保存在 cookie 中，并且攻击者无法伪造的数据作为校验，那么攻击者就无法再运行 CSRF 攻击。

## **令牌同步模式（token）**

- 由服务端生成一个伪随机、且加密的 token(令牌)，可以针对**每个用户会话**或针对**每个请求**生成一次，然后发送给客户端
- 当客户端发出请求时，携带该令牌用以服务器端校验，只有和服务端存储在 session 中的令牌一致，才正常响应，否则拒绝这次请求。
  > 如何操作？之后在每次页面加载时，使用 JS 遍历整个 DOM 树，对于 DOM 中所有的 a 和 form 标签后加入 Token。这样可以解决大部分的请求，但是对于在页面加载之后动态生成的 HTML 代码，这种方法就没有作用，还需要程序员在编码时手动添加 Token。

需要注意的是：

- **CSRF 令牌不应使用 Cookie 进行传输(防范 xss 攻击)，而是--**
- **在**隐藏字段 form 参数中添加令牌--

```javascript
<form action="/transfer.do" method="post">
  <input type="hidden" name="CSRFToken" value="tokenVal">
</form>
```

或者

- **在自定义 HTTP 请求标头中插入 CSRF 令牌**

### 令牌同步模式的缺点：

- 在会话中存储 Token 比较繁琐
- 对于大型网站来说，需要生成和存储的 token 过多，会增加服务器负担。
- 对于分布式、做了负载均衡(如 Nginx)的网站，由于 session 不能在多台服务器间共享，因此无法使用该方法

## 双重 Cookie 验证

- 在用户登录后，由客户端生成一个令牌 token(加密，且伪随机)，
- 将其放入 cookie 中
- 在请求时，携带 Cookie 一起发送，并将 token 作为参数拼接在 url 后
- 后端校验发送过来的 Cookie 中的 token 和 url 后的参数是否相等
- 要求每个请求都将此 token 作为隐藏表单值
  > > 深度防御技术(后端)

## SameSite Cookie 属性

> > 为了从源头上解决这个问题，Google 起草了一份草案来改进 HTTP 协议，那就是为 Set-Cookie 响应头新增 Samesite 属性，它用来标明这个 Cookie 是个“同站 Cookie”，同站 Cookie 只能作为第一方 Cookie，不能作为第三方 Cookie，Samesite 有两个属性值，分别是 Strict 和 Lax，下面分别讲解：

SameSite 是一个 cookie 响应头的属性（类似于 HTTPOnly 等）

- 此属性可帮助浏览器确定是否将 cookie 与跨站点请求一起发送。
- 此属性可能的值`Lax`，`Strict`或`None`。

### `Strict`：

表明这个 Cookie 在任何情况下都不可能作为第三方 Cookie

> > 举个栗子

比如说 b.com 设置了如下 Cookie：

```javascript
Set-Cookie: foo=1; Samesite=Strict
Set-Cookie: bar=2; Samesite=Lax
Set-Cookie: baz=3
```

我们在 a.com 下发起对 b.com 的任意请求，foo 这个 Cookie 都不会被包含在 Cookie 请求头中，但 bar 会。
举个实际的例子就是，假如淘宝网站用来识别用户登录与否的 Cookie 被设置成了 Samesite=Strict，那么用户从百度搜索页面甚至天猫页面的链接点击进入淘宝后，淘宝都不会是登录状态，因为淘宝的服务器不会接受到那个 Cookie，其它网站发起的对淘宝的任意请求都不会带上那个 Cookie。

---

### `Lax`：

- 这种称为宽松模式，比 Strict 放宽了点限制：假如这个请求是这种请求（改变了当前页面或者打开了新页面）且同时是个 GET 请求，则这个 Cookie 可以作为第三方 Cookie。
- 用户在不同网站之间通过链接跳转是不受影响了。
  - 但假如这个请求是从 a.com 发起的对 b.com 的**异步请求，**或者页面跳转是通过表单的**post 提交**触发的，则也不会携带该 cookie

参考：

[前端安全系列之二：如何防止 CSRF 攻击--思否](https://segmentfault.com/a/1190000016659945?fileGuid=XvJTqWPtYtDckDt8)

[前端安全系列之二：如何防止 CSRF 攻击](https://www.cnblogs.com/lr393993507/p/9834856.html?fileGuid=XvJTqWPtYtDckDt8)

[跨站点请求防伪备忘单](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html#synchronizer-token-pattern?fileGuid=XvJTqWPtYtDckDt8)

[跨站请求伪造](https://zh.wikipedia.org/wiki/%E8%B7%A8%E7%AB%99%E8%AF%B7%E6%B1%82%E4%BC%AA%E9%80%A0?fileGuid=XvJTqWPtYtDckDt8)
