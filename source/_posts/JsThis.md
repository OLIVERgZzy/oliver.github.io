---
title: 读《你不知道的JavaScript》笔记之：this
top: false
cover: /images/5XQk9gFlIAB2z3m.jpg
toc: false
mathjax: false
updated: 2020-09-26 10:55:02
top_img: /images/5XQk9gFlIAB2z3m.jpg
description: this 关键字是 JavaScript 中最复杂的机制之一。搞懂它有助于驾驭 JavaScript 这匹野马。
tags:
  - JavaScript
  - this
categories:
  - 前端
  - 读书笔记
keywords: 前端, javascript, this, this关键字, this指向, 上下文, call, apply, bind, 你不知道的javascript
---

# 前言

最近喜欢利用思维导图这个强大的工具，来梳理和提练知识要点。用的是 [百度脑图](https://naotu.baidu.com/)这个工具，应该是百度目前唯一一款良心产品了。😂 下面我按照思维导图的脉络，去把这个知识点来说清楚吧。（思维导图单击可放大查看）

![this思维导图.png](/images/this思维导图.svg)

# 一、关于 this

this 关键字是 JavaScript 中最复杂的机制之一。搞懂它有助于驾驭 JavaScript 这匹野马。

# 二、对 this 错误的认识

在我刚从后端转前端初期那段时间，我也一样对 this 有这两大错误认识，这可能是就是 this 最具迷惑性的地方吧。在理解 this 是如何工作之前，我们就先消除这两种对于 this 的错误认识吧。

## 2.1 指向自身

也许是受 Java 语言的影响，在我刚开始学习前端的初期也误以为 JavaScript 的 this 关键字和 Java 中的 this 关键字功能类似。

让我来回忆一下 this 关键字在 Java 中有哪些用法吧：

1. 关键字可用来引用当前类的实例变量
2. 关键字可用于调用当前类方法
3. 可以用来调用当前类的构造函数

我在编写 JavaScript 代码初期，认为既然 JavaScript 中函数可以被看作是一个对象，那么在函数中使用 this 从内部引用函数自身也是可行的，顺着这个惯性思维我写下了如下代码：

```javascript
function foo() {
  this.count++;
}

foo.count = 0;

for (let i = 0; i < 5; i++) {
  foo();
}

console.log(foo.count); // 0
```

虽然表面上看 foo 被调用了 4 次，但是实际上 foo.count 仍然是 0。所以看来 this.count 和 foo.count 指向的并不是同一个变量。

在执行 foo.count = 0; 时会在函数对象 foo 添加一个属性 count，而 this.count 则在全局作用域中创建 count 变量，它的值是 NaN。

那么如何解决这种问题呢？

1. 通过词法作用域解决（这种方法虽然有效，但是回避了 this）：

```js
function foo() {
  data.count++;
}

var data = {
  count: 0,
};

for (let i = 0; i < 5; i++) {
  foo();
}

console.log(data.count); // 5
```

2. 通过具名函数解决：

```js
function foo() {
  foo.count++;
}

foo.count = 0;

for (let i = 0; i < 5; i++) {
  foo();
}

console.log(foo.count); // 5
```

3. 通过 call 解决：

```js
function foo() {
  this.count++;
}

foo.count = 0;

for (let i = 0; i < 5; i++) {
  foo.call(foo, i);
}

console.log(foo.count); // 5
```

## 2.2 联通作用域

第二种常见错误是，this 指向函数的作用域。

```js
function foo() {
  var a = 2;
  this.bar();
}

function bar() {
  console.log(this.a);
}

foo(); // undefined 或 TypeError: this.bar is not a function
```

这段代码具有误导性，企图在 foo 函数中调用 bar（浏览器环境下可以调通，Node 环境下报错），又因为 bar 是在 foo 函数内部中被调用，所以从写法角度去看，会造成在 bar 函数中可以访问到变量 a 的假象，但是实际上并不能。

以上就是学习 this 最容易犯的两大错误认识。

# 三、this 工作机制

在消除对 this 误解之后，我们来探寻 this 真正的工作机制到底是什么。

每个函数的 this 是在调用时被绑定的，完全取决于函数的调用位置（也就是函数的调用方法）

# 四、理解 this 绑定过程

## 4.1 寻找函数被调用的位置

寻找函数被调用的位置，最重要的就是分析调用栈（就是为了到达当前执行位置所调用的所有函数）。调用位置就在当前正在执行的函数的前一个调用中。

用代码来表示：

```js
function baz() {
  // 当前调用栈：baz
  // 因此，当前调用位置是全局作用域
  console.log("baz");

  bar(); // <--- bar 的调用位置
}

function bar() {
  // 当前调用栈：baz --> bar
  // 因此，当前调用位置在baz中
  console.log("bar");

  foo(); // <--- foo 的调用位置
}

function foo() {
  // 当前调用栈：baz --> bar --> foo
  // 因此，当前调用位置在bar中
  console.log("foo");

  foo(); // <--- bar 的调用位置
}

baz(); // <-- baz 的调用位置
```

## 4.2 判断绑定规则

在找到函数被调用的位置之后，接下来就可以根据优先级判断适用的绑定规则。
以下绑定规则按优先级由高到低排列。

### 4.2.1 new 绑定

使用 new 来调用函数，或者发生构造函数调用时，会自动执行下面这些操作。

1. 创建（构造）一个全新的对象。
2. 这个新对象会执行 \[\[Prototype\]\]连接。
3. 这个新对象会绑定到函数调用的 this。
4. 如果函数没有返回其他对象，那么 new 表达式中的函数调用会自动返回这个新对象。

```js
function foo(a) {
  this.a = a;
}

var bar = new foo(2);
console.log(bar.a); // 2
```

使用 new 调用 foo(...) 时，会构造一个新对象并把它绑定到 foo(...)调用中的 this 上。

### 4.2.2 显示绑定

JavaScript 提供了两个方法可以强制在某个对象上调用函数，call(...) 和 apply(...) 方法。

```js
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
};

foo.call(obj); // 2
```

通过 foo.call(...)，我们可以在调用 foo 时强制把它的 this 绑定在 obj 上。

### 4.2.3 隐式绑定

```js
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
  foo: foo,
};

obj.foo(); // 2 (调用位置)
```

上面这段代码，调用位置使用 obj 上下文来引用函数，可以说成 obj 对象“拥有”或者“包含”函数引用。

当函数引用有上下文对象时，隐式绑定规则会把函数调用中的 this 绑定到这个上下文对象中。

对象属性引用链中只有最后一层在调用位置中起作用。举个例子：

```js
function foo() {
  console.log(this.a);
}

var obj2 = {
  a: 42,
  foo: foo,
};

var obj1 = {
  a: 2,
  obj2: obj2,
};

obj1.obj2.foo(); // 42 (调用位置)
```

### 4.2.4 默认绑定

最常用的函数调用类型：独立函数调用。

```js
function foo() {
  // 当前调用栈是：foo
  // 因此，当前调用位置是全局作用域
  console.log(this.a);
}

var a = 2;

foo(); // 2
```

上面这段代码，foo() 的调用的位于全局作用域中，且 foo() 是直接使用不带任何修饰的函数引用进行调用的，因此适用于默认绑定。

在严格模式下，则不能将全局对象用于默认绑定，因此 this 会绑定到 undefined。

## 4.3 绑定例外

### 4.3.1 被忽略的 this

当 null 或者 undefined 作为 this 的绑定对象传入 call、apply 或者 bind，这些值在调用时会被忽略，实际应用的是默认规定规则:

```js
function foo() {
  console.log(this.a);
}

var a = 2;

foo.call(null); // 2
```

### 4.3.2 间接引用

调用函数的“间接引用”会应用默认绑定规则。

```js
function foo() {
  console.log(this.a);
}

var a = 2;
var o = { a: 3, foo: foo };
var p = { a: 4 };

o.foo(); // 3
(p.foo = o.foo)(); // 2
```

复制表达式 `p.foo = o.foo` 的返回值是目标函数的引用，因此调用位置是 foo() 而不是 p.foo() 或者 o.foo()。

# 五、注意

## 5.1 箭头函数

在 ES6 中规范中的箭头函数，完全不适用于四条绑定规则，而是根据当前词法作用域来决定 this。

## 5.2 编码风格

在编码时，词法作用域风格与 this 风格代码不要混用，同时使用会使代码更加难以维护，并且可能也会更难编写。

# 六、总结

如果要判断一个运行中的函数的 this 绑定，就需要找到这个函数的直接调用位置。找到之后就可以顺序应用下面这四条规则来判断 this 的绑定对象。

1. 由 new 调用？绑定到新创建的对象。
2. 由 call 或者 apply（或者 bind）调用？绑定到指定的对象。
3. 由上下文对象调用？绑定到那个上下文对象。
4. 默认：在严格模式下绑定到 undefined，否则绑定到全局对象。
