---
title: 读《你不知道的JavaScript》笔记之：this
author: Oliver
top: false
cover: false
toc: false
mathjax: false
date: 2020-09-26 10:55:02
img: /images/5XQk9gFlIAB2z3m.jpg
password:
summary: this 关键字是 JavaScript 中最复杂的机制之一。搞懂它有助于驾驭 JavaScript 这匹野马。
tags:
  - JavaScript
  - this
categories:
  - 前端
  - 读书笔记
keywords: 前端, javascript, this, this指向, 上下文, call, apply, bind, 你不知道的javascript
---

# 前言

最近喜欢利用思维导图这个强大的工具，来梳理和提练知识要点。用的是 [百度脑图](https://naotu.baidu.com/)这个工具，应该是百度目前唯一一款良心产品了。😂 下面我按照思维导图的脉络，去把这个知识点来说清楚吧。（思维导图单击可放大查看）

![this思维导图.png](/images/this思维导图.svg)

# 一、关于 this

this 关键字是 JavaScript 中最复杂的机制之一。搞懂它有助于驾驭 JavaScript 这匹野马。

# 二、对 this 错误的认识

在我刚从后端转前端初期那段时间，我也一样对 this 有这两大错误认识，这可能是就是 this 最具迷惑性的地方吧。在理解 this 是如何工作之前，我们就先消除这两种对于 this 的错误认识吧。

# 2.1 指向自身

也许是受 Java 语言的影响，在我学习前端的初期也误以为 JavaScript 的 this 关键字和 Java 中的 this 关键字功能一样。

在 Java 中 this 关键字的用法大概如下：

1. 关键字可用来引用当前类的实例变量
2. 关键字可用于调用当前类方法
3. 可以用来调用当前类的构造函数

# 2.2 联通作用域
