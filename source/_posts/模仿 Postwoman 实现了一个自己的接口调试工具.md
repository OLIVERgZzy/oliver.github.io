---
title: 模仿 Postwoman 实现了一个自己的接口调试工具
author: Oliver
top: false
cover: true
toc: false
mathjax: false
date: 2020-08-22 23:30:43
img: /images/WX20200822-223911@2x.png
password:
summary:
tags:
  - Nuxt
  - Vue
  - Http协议
categories:
  - 前端
  - 我的项目
keywords: api-client, request-client, axios, postman, postwoman, nuxt, vue, vuejs, nuxtjs, 代替postman, 接口测试, 接口调试, API测试, 接口调试工具
---

# 1. 前言

想必大家平日里开发接口，或多或少都用过 postman 这款接口测试工具，应该都对他不陌生。近日偶然发现一款 web 版接口测试工具并且免费开源。对，它就是 [postwoman](https://postwoman.io/)，清爽帅气的 UI 界面，强大的功能，相对 postman 丝毫不逊色，令我啧啧称赞。

# 2. 动机

出于学习的目的，我也模仿开发了一个精简版接口测试工具并起名为 [postchild](http://postchild.io)。
既然是学习的目的，在此次项目中我会尽可能的减少第三方库的使用，尽量靠自己写代码去实现。在过程，也是一种对知识的查漏补缺，尤其是我对 web 通讯、http 协议这方面的不足。下面将会简单介绍一下运用到的技术栈和对未来打算实现的功能。

# 3. 技术栈

1. vue
2. nuxt
3. axios

# 4. 功能清单

- [x] 基本请求 GET, HEAD, POST, PUT, DELETE, OPTIONS, PATCH
- [x] 历史记录(vuex 实现)
- [ ] WebSocket
- [ ] Socket.IO
- [ ] 界面主题色切换
- [ ] 授权登录(为了储存历史记录和收藏夹)
- [ ] 收藏夹功能
- [ ] gRPC 请求

# 5. 体验地址

最后再发一下体验地址: [http://postchild.io](http://postchild.io)
