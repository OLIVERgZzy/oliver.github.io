---
title: 读《你不知道的JavaScript》笔记之：作用域
author: Oliver
top: false
cover: false
toc: false
mathjax: false
date: 2020-06-29 23:30:43
img: /images/5XQk9gFlIAB2z3m.jpg
password:
summary: 恰好最近读了《你不知道的 JavaScript》这本书，书中的第一章节就介绍了 JavaScript 的作用域，作者用着非常生动的文字和拟人的手法，描述了 JavaScript 执行过程中：引擎、编译器和作用域分别扮演的不同角色与职责。
tags:
  - JavaScript
  - 作用域
categories:
  - 前端
  - 读书笔记
keywords: 前端, javascript, 作用域, javascript作用域, javascript编译原理, 你不知道的javascript
---

# 前言

作用域在 JavaScript 中是个一直是个老生长谈的话题，因为涉及到到原理非常深，知识点非常广，困扰着无数新手前端，我就是其中一个。恰好最近读了《你不知道的 JavaScript》这本书，书中的第一章节就介绍了 JavaScript 的作用域，作者用着非常生动的文字和拟人的手法，描述了 JavaScript 执行过程中：引擎、编译器和作用域分别扮演的不同角色与职责。
（思维导图单击可放大查看）

![作用域思维导图.png](/images/作用域思维导图.svg)

# 一、关于作用域

作用域几乎是所有编程语言最基本的功能之一，其基本功能就是储存变量中的值，并且能在之后对这个值进行访问或修改。

# 二、编译原理

---

了解过编译原理的同学可能都会知道，大多数编程语言在执行前，一般要经过：词法分析、语法分析、代码优化、生成中间码或生成指定平台的 CPU 指令（这里对编译的流程的解释过于粗略，后面打算专门几篇文章来梳理编译原理）。

那么为什么我要简单聊一下编译过程呢？这就和 JavaScript 语言的特殊性息息相关了，我们通常把 JavaScript 认为是“解释型”语言，但归根结底它还是一门编译语言，只不过它的某些环节变的相当复杂。比如，它的构建过程是在执行前的几微秒内发生的（甚至更短），所以 JavaScript 引擎用尽各种方法（比如 JIT，可以延迟编译）来保证性能最佳。

简单来说，任何 JavaScript 代码片段在执行前都需要进行编译（通常就是在执行前）。那么你可能会问了，这个高深莫测的编译原理和我今天讨论的主题有什么关联呢？我们先怀揣着疑问，继续看下去。

# 三、理解作用域

---

## 3.1 嘉宾表

下面邀请出今晚最重要的三位嘉宾，它们分别是：引擎、编译器和作用域，掌声欢迎。

我叫引擎，执行 JavaScript 可少不了我的存在，我负责 JavaScript 编译、优化、执行的全过程。

我叫编译器，是引擎大哥的最重要的助手，在接到引擎大哥交给我的 JavaScript 代码片段之后，我会立刻将其转换成 AST（抽象语法树）。在把 AST 交给引擎大哥前，我还会与作用域先生（那个家伙脾气可不太好）一起协调确认作用域规则。

我叫作用域，是引擎的另一位好朋友，负责收集并维护由所有声明的标识符（变量）组成的一系列查询，并实施一套非常严格的规则。不过我的脾气比较粗暴，因为编译器总是会问我，“你有没有见过变量 a ”、“你有没有见过函数 foo ”等之类的问题，通常如果我见过就会返回这个变量，如果从来没见过的话，就会给编译器脸色看（抛出一个异常）。

## 3.2 引擎查找变量规则

在正式理解作用域之前，我们得先了解一下这个概念————编译器的 LHS RHS，也就是 JavaScript 是如何变量声明和赋值。

当出现赋值操作时，编译器会使用 LHS 方法沿着作用域链，从当前作用域到顶层作用域中挨个查询目标，如果一直没有找到该目标，则会抛出异常（ReferenceError）程序就不会再往下执行，请看下面的例子：

```js
var a = 1;
```

当引擎去执行这段代码的时候，其实分成了两步：
第一步：编译器在把代码转换成 AST 树后，引擎会为 a 当前所在的作用域内开辟一个名为 a 的容器（通常作用域在执行前确定）。
第二步：接着编译器会使用 LHS 规则去查找这个名为 a 的容器，最后给这个名为 a 的容器赋值。

当出现取值操作时，编译器会使用 RHS 方法沿着作用域链，从当前作用域到顶层作用域中挨个查询目标，如果一直没有找到该目标，则会跑出异常（ReferenceError）程序就不会再往下执行，请看下面的例子：

```js
var a = 1;

var foo = function (b) {
  console.log(b);
};

foo(a);
```

从 `foo(a);` 这行代码开始，首先会按 RHS 规则查询 foo，如果查询成功，则继续查询 a，如果查询成功将 a 的值映射到 foo 函数的 arguments[0] ，于是执行 `console.log(b);`。其实这里的 `console.log` 也会按照 RHS 的规则查找当前作用域中是否存在 `console`，以此类推，继续查找 `console.log`;

## 四、作用域嵌套

作用域还存在作用域嵌套的问题，比如 `foo` 就嵌套在全局作用域当中，`boo` 嵌套在 `foo` 的函数作用域中，我们看下面这段代码：

```js
var a = 1;

var foo = function (a) {
  var b = a * 3;
  var c = 3;

  function boo(a, b) {
    var c = b * 3;
    console.log(a, b, c);
  }

  boo(a, b);
};

foo(a);
```

这段代码的执行结果最终是什么呢？答案是 `1, 3, 9`, 虽然 foo 函数内部和 boo 函数内部都有名称为 c 的变量名，但是根据作用域的规则，它总是取离他最近的那个变量。所以外层的 c 会被里层的 c 所覆盖。但是在严格模式下，这种行为不被允许。

# 五、小结

- 通常作用域在 JavaScript 引擎执行代码片段前，且一经确认则不会再发生改变
- 作用域与 JavaScript 编译器的两种查询规则紧密配合： LHS、RHS
- 作用域存在作用域嵌套问题，变量同名的情况下，内部的优先级高于外部的优先级