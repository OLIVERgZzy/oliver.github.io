---
title: TypeScript 基础语法
author: Oliver
img: https://i.loli.net/2020/06/26/FXahC9M3wfxZtjO.jpg
top: false
cover: true
toc: true
mathjax: false
date: 2020-06-26 21:36:33
password:
summary: 用代码示例带你一步一步了解 TypeScript 基础语法知识
tags: TypeScript
categories: 前端
keywords: typescript,typescript中文教程,typescript语法,typescript enum,typescript array,typescript interface
---

# 一、开始一个项目

## 1.1 安装模块

```bash
# 初始化项目
npm init -y

# 全局安装 ts
npm i -g typescript

# 初始化ts配置文件
tsc --init

# 安装 webpack
npm i webpack webpack-cli webpack-dev-server -D

# 安装 ts-loader 再次在当前项目中安装 typescript
npm i ts-loader typescript -D

# 安装  html-webpack-plugin 当使用 webpack打包时，创建一个 html 文件，并把 webpack 打包后的静态文件自动插入到这个 html 文件当中。
npm install html-webpack-plugin -D

# 安装 clean-webpack-plugin -D 可以帮助我们清空打包时产生的无用文件
npm i clean-webpack-plugin -D

# 安装 webpack-merge -D 可以帮助我们合并多个文件
npm i webpack-merge -D
```

## 1.2 配置 package.json

```json
"scripts": {
  "start": "webpack-dev-server --mode=development --config ./build/webpack.config.js", // 开发环境启动脚本
  "build": "webpack --mode=product --config ./build/webpack.config.js" // 构建生产环境脚本
}
```

# 二、对比 TS 数据类型与 ES6 数据类型

---

| ES6 的数据类型 | TypeScript 的数据类型 |
| -------------- | --------------------- |
| Boolean        | Boolean               |
| Number         | Number                |
| String         | String                |
| Array          | Array                 |
| Function       | Function              |
| Object         | Object                |
| Symbol         | Symbol                |
| undefined      | undefined             |
| null           | null                  |
|                | **void**              |
|                | **any**               |
|                | **never**             |
|                | **元组**              |
|                | **枚举**              |
|                | **高级类型**          |

# 三、数据类型

---

## 3.1 基本类型

```typescript
// 原始类型
let bool: boolean = true;
let num: number = 123;
let str: string = "abc";

// 数组
let arr1: number[] = [1, 2, 3]; // 只能存number
// 泛型：联合类型 可以存 number 和 string
let arr2: Array<number | string> = [1, 2, 3, "4"];

// 元组 只能存放 两个类型一致的元素（实际开发中不建议使用）
let tuple: [number, string] = [0, "1"];
tuple.push(2); // [0, "1", 2]
tuple[2]; //  ERROR：无法访问

// 函数
let add = (x: number, y: number): number => x + y;
// 函数定义
let compute: (x: number, y: number) => number;

// 对象
let obj: object = { x: 1, y: 2 };
obj.x = 3; //  ERROR：不能分配属性

let obj: { x: number; y: number } = { x: 1, y: 2 };
obj.x = 3; // ok

// symbol
let s1: symbol = Symbol();
let s2 = Symbol();
console.log(s1 === s2); // fasle

// undefined, null
let un: undefined = undefined;
let empty: null = null;
un = 3; // 不允许
empty = 3; // 不允许

// void 避免 undefined值被污染
// js中 undefined 可以被设置为一个指定的值
let noReturn = () => {};

// any
let x;
x = 1;
x = [];
x = () => {};

// never
// 永远不会返回
let error = () => {
  throw new Error("error");
};
let endless = () => {
  while (true) {}
};
```

## 3.2 枚举类型

```typescript
// 数字枚举
enum Role {
  Developer = 1,
  Guest,
}
console.log(Role.Developer); // 1
console.log(Role); // { "Developer": 1, "Guest": 2, 1: "Developer", 2: "Guest"}
Role.Guest = 3; //  ERROR：无法修改

// 字符串枚举
enum Message {
  Success = "成功",
  Fail = "失败",
}

// 异构枚举（不建议使用）
enum Answer {
  N = 1,
  Y = "yes",
}

// 枚举成员
enum Char {
  a = 1,
  b = Char.a,
  c = 1 + 3,

  d = Math.random(),
  e = "123".length,
  f, //  ERROR：必须具有初始值
}

// 常量枚举
// 不需要枚举对象，而只需要使用枚举对象的值时候，可以使用
const enum Month {
  Jan,
  Feb,
  Mar,
}

// 枚举类型
enum E {
  a,
  b,
}
enum F {
  a = 0,
  b = 1,
}
enum G {
  A = "apple",
  b = "banana",
}

let e: E = 3;
let f: F = 3;
e === f; //  ERROR：不同枚举类型不可比较

let e1: E.a;
let e2: E.b;
e1 === e2; // ERROR：不同枚举类型不可比较

let e3: E.a = 1;
e1 === e3; // ok

let g1: G = G.b;
let g2: G.a = G.a;
```

## 3.3 接口类型

### 3.3.1 对象类型接口

```typescript
// 定义接口 List
interface List {
  id: number;
  name: string;
}

// 定义接口 Result
interface Result {
  data: List[];
}

function render(result: Result) {
  result.data.forEach((value) => {
    console.log(value.id, value.name);
  });
}

let result = {
  data: [
    { id: 1, name: "A" },
    { id: 2, name: "B" },
    { id: 3, name: "C", sex: "male" }, // 增加的 sex 不会报错，符合鸭式变形法
  ],
};

render(result);
```

#### 3.3.1.1 鸭式变形法

> 只要传入的数据格式满足接口定义的必要条件也是可以允许的。

当然也有例外，请看下面：

```typescript
render({
  data: [
    { id: 1, name: "A" },
    { id: 2, name: "B" },
    { id: 3, name: "C", sex: "male" },
  ],
}); // ERROR：如果直接传入对象自变量，TS则会对额外的字段进行类型检查
```

#### 3.3.1.2 有三种绕过方式

1. 加类型断言
2. 用变量传递
3. 索引签名

```typescript
// 方法1 加类型断言
render({
  data: [
    { id: 1, name: "A" },
    { id: 2, name: "B" },
    { id: 3, name: "C", sex: "male" },
  ],
} as Result);

// 方法2 用变量传递
let result = {
  data: [
    { id: 1, name: "A" },
    { id: 2, name: "B" },
    { id: 3, name: "C", sex: "male" },
  ],
};
render(result);

// 方法3 索引签名
interface List {
  id: number;
  name: string;
  [x: string]: any; // 含义，用任意的字符串去索引 List，这样可以支持多个属性
}
```

#### 3.3.1.3 假设有个新需求

> 需要判断一个对象中是否有一个新字段（age），如果有则打印出来

```typescript
// 定义接口 List
interface List {
  readonly id: number; // 含义，只读，不可修改
  name: string;
  age?: number; // 含义，可选属性，这个属性可以有也可以没有
}

// 定义接口 Result
interface Result {
  data: List[];
}

function render(result: Result) {
  result.data.forEach((value) => {
    console.log(value.id, value.name);
    if (value.age) {
      console.log(value.age);
    }
    value.id++; // 报错，不可修改
  });
}

let result = {
  data: [
    { id: 1, name: "A" },
    { id: 2, name: "B" },
    { id: 3, name: "C", age: 23 },
  ],
};

render(result);
```

#### 3.3.1.3 索引签名

> 我们可以明确的指定索引签名。例如：假设你想确认存储在对象中任何内容都符合  { message: string }  的结构，你可以通过  [index: string]: { message: string }  来实现。

```typescript
interface foo {
  [index: string]: { message: string };
}

// 储存的东西必须符合结构 ok
foo["a"] = { message: "some message" };

// ERROR：必须包含 `message`
foo["a"] = { abc: "some message" };
```

**tip：**

> [index: string]: { message: string }; 里的  index 除了可读性外，并没有任何意义。例如：如果有一个用户名，你可以使用  { username: string}: { message: string }，这有利于下一个开发者理解你的代码。

节选自：[深入理解 TypeScript - 索引签名](https://jkchao.github.io/typescript-book-chinese/typings/indexSignatures.html#typescript-%E7%B4%A2%E5%BC%95%E7%AD%BE%E5%90%8D)

### 3.3.2 函数类型接口

#### 3.3.2.1 声明

```typescript
// 声明函数的四种方式

// 函数定义
function add(x: number, y: number) {
  return x + y;
}

// 变量定义函数类型
let add: (x: number, y: number) => number;

// 接口定义函数类型
interface Add {
  (x: number, y: number): number;
}

// 类型别名定义函数类型
type Add = (x: number, y: number) => number;

// 定义
let add: Add = (a, b) => a + b;
```

#### 3.3.2.2 可选参数

```typescript
function add(x: number, y?: number) {
  return y ? x + y : x;
}

console.log(add(1)); // 1
console.log(add(1, 2)); // 3

// ERROR：必选参数不能位于可选参数之后
function add(x: number, y?: number, z: number) {
  return y ? x + y : x;
}
```

#### 3.3.2.3 默认值

```typescript
function add(x: number, y = 0, z: number, q = 1) {
  return y + x + z + q;
}

console.log(add(1, undefined, 3)); // 5
```

#### 3.3.2.4 剩余参数

```typescript
function add(x: number, ...rest: number[]) {
  return (
    x +
    rest.reduce((pre, cur) => {
      return pre + cur;
    })
  );
}

console.log(add(1, 2, 3, 4, 5)); // 15
```

#### 3.3.2.5 函数重载

```typescript
function add(...rest: number[]): number;
function add(...rest: string[]): string;
function add(...rest: any[]): any {
  let first = rest[0];
  if (typeof first === "string") {
    return rest.join("");
  }
  if (typeof first === "number") {
    return rest.reduce((pre, cur) => pre + cur);
  }
}

console.log(add(1, 2, 3)); // 6
console.log(add("a", "b", "c")); // "abc"
```

## 3.4 类

### 3.4.1 基本知识

```typescript
class Dog {
  constructor(name: string) {
    this.name = name;
  }
  name: string; // 必须在构造函数中被初始化，默认 public
  run() {}
  private pri() {}
  protected pro() {}
  readonly legs: number = 4; // 只读，必须给初始值
  static food: string = "bones"; // 只能通过类名调用
}

// 实例化 Dog
let dog = new Dog("wangwang");
console.log(dog);
dog.pri(); // ERROR：无法调用私有属性
dog.pro(); // ERROR: 无法调用
console.log(Dog.food); // 'bones'

class Husky extends Dog {
  constructor(name: string, color: string) {
    super(name); // 必须加上

    // this 必须在 super 之后调用
    this.color = color;
    this.pri(); // ERROR：无法调用私有属性
    this.pro(); // OK
  }
  color: string; // 默认 public
}
console.log(Husky.food); // 'bones'
```

> 构造函数设置为 protected，说明这个类只能被继承，而不能被实例化。
> 构造函数设置为 private，说明这个类不能被继承，不能被实例化。

### 3.4.2 抽象类——多态特性

```typescript
abstract class Animal {
  eat() {
    console.log("eat");
  }
  abstract sleep(): void;
}
// let animal = new Animal(); // Error：无法实例化

class Dog extends Animal {
  constructor(name: string) {
    super();
    this.name = name;
  }
  name: string;
  run() {}
  sleep() {
    console.log("dog sleep");
  }
}

class Cat extends Animal {
  sleep() {
    console.log("cat sleep");
  }
}

let dog = new Dog("wangcai");
let cat = new Cat("dema");

let animals: Animal[] = [dog, cat];
animals.forEach((i) => {
  i.sleep();
  /**
   * dog sleep
   * cat sleep
   */
});
```

### 3.4.3 链式调用

```typescript
class WorkFlow {
  step1() {
    return this;
  }
  step2() {
    return this;
  }
}

new WorkFlow().step1().step2(); // ok

class MyFlow extends WorkFlow {
  next() {
    return this;
  }
}

new MyFlow().next().step1().step2(); // ok
```

## 3.5 类与接口的关系

![类与接口的关系.jpg](https://i.loli.net/2020/06/26/doJnDwsM3qIr7O1.jpg)

### 3.5.1 接口只能约束类的公有成员

```typescript
interface Human {
  name: string;
  eat(): void;
}

class Asian implements Human {
  constructor(name: string) {
    this.name = name;
  }
  name: string;
  eat() {} // 必须实现
  sleep() {} // 必须实现
}
```

### 3.5.2 一个类类型接口可以继承多个接口

```typescript
interface Man extends Human {
  run(): void;
}

interface Child {
  cry(): void;
}

interface Boy extends Man, Child {}

let boy: Boy = {
  name: "",
  run() {},
  eat() {},
  cry() {},
};
```

### 3.5.3 接口继承类

```typescript
class Auto {
  state = 1;
}

interface AutoInterface extends Auto {}

class C implements AutoInterface {
  state = 1;
}
```

# 四、泛型

---

## 4.1 泛型函数与泛型接口

```typescript
// 泛型函数
function log<T>(value: T): T {
  console.log(value);
  return value;
}

log<string[]>(["a", "b"]);
log(["a", "b"]);

// 类型定义
type Log = <T>(value: T) => T;

// 泛型接口定义
interface Log<T = string> {
  (value: T): T;
}
```

## 4.2 泛型类与泛型约束

```typescript
// 泛型类
class Log<T> {
  run(value: T) {
    console.log(value);
    return value;
  }
}

// 指定number类型
let log1 = new Log<number>();
log1.run(1);

// 指定string类型
let log2 = new Log<string>();
log2.run("1");

// 不指定类型
let log2 = new Log();
log2.run("1");
log2.run(1);
```

```typescript
// 定义接口
interface Length {
  length: number;
}

// 继承Length，只能传入有length的属性的值
function log<T extends Length>(value: T): T {
  console.log(value, value.length);
  return value;
}

log([1]);
log("123");
log({ length: 1 });
```

## 4.3 泛型的好处

1. 函数和类可以轻松地支持多种类型，增强程序的拓展性
2. 不必写多条函数重载，冗长的联合类型声明，增强代码可读性
3. 灵活控制类型之前的约束

# 五、类型检查机制

---

## 5.1 自动类型推断

```typescript
let a = 1; // 自动推断a为number

let b = [1, null]; // 自动推断 b 为 []any
```

## 5.2 类型兼容性

##### 当一个类型 Y 可以被赋值给另一个类型 X 时，我们就可以说类型 X 兼容类型 Y X 兼容 Y : X (目标类型) = Y（源类型）

##### 口诀：

1. 结构之间兼容：成员少的兼容成员多的
2. 函数之间兼容：参数多的兼容参数少的

### 5.2.1 变量兼容性

```typescript
let s: string = "a";
s = null;
```

### 5.2.2 接口兼容性

```typescript
// 接口兼容性
interface X {
  a: any;
  b: any;
}

interface Y {
  a: any;
  b: any;
  c: any;
}

let x: X = { a: 1, b: 2 };
let y: Y = { a: 1, b: 2, c: 3 };
x = y; // ok
// y = x; // Error 不兼容
```

> 结论：属性少的兼容属性多的

### 5.2.3 函数兼容性

```typescript
/**
  * 结论：参数多的兼容参数少的
  */

// 函数兼容性
type Handler = (a: number, b: number) => void
function hor(handler: Handler) {
    return handler
}

// 1. 参数个数
let handler1 = (a: number) => {};
hor(hander1);
let handler2 = (a: number, b: number, c: number) => {};
// hor(handler2); // Error 超出形参个数

// 2. 可选参数和剩余参数
let a = (p1: number, p2: number) => {};
let b = (p1?: number, p2?: number) => {};
let c = (...args: number[]) => {};

a = b; // OK
a = c; // OK
// b = c; // Error
// b = a; // Error

// 通过修改 tsconfig.json >> "strictFunctionTypes": false
b = c; // OK
b = a; // OK
c = a; // OK
c = b; // OK

// 3. 参数类型
let handler3 = (a: string) => {};
// hor(handler3); // Error 类型不匹配

interface Point2D {
    x: number;
    y: number;
}
interface Point3D {
    x: number;
    y: number;
    z: number;
}
let p2d = (point: Point2D) => {};
let p3d = (point: Point3D) => {};
p3d = p2d; // ok
// p2d = p3d: // Error

// 通过修改 tsconfig.json >> "strictFunctionTypes": false
p2d = p3d: // OK

// 4. 返回值类型
let f = () => ({name: 'Alice'});
let g = () => ({name: 'Alice', sex: 'man'});
f = g;
// g = f; // Error 不满足

// 函数重载兼容
function overload(a: number, b: number): number;
function overload(a: string, b: string): string;
function overload(a: any, b: any): any;
```

### 5.2.4 枚举兼容性

```typescript
// 枚举兼容性
enum Fruit {
  Apple,
  Banana,
}
enum Color {
  Red,
  Yellow,
}
let fruit: Fruit.Apple = 3;
let no: number = Fruit.Apple;
// let color: Color.Red = Fruit.Apple; // Error 不兼容
```

### 5.2.5 类兼容性

```typescript
// 类兼容性
class A {
    constructor(p: number, q: number) {}
    id: number = 1
}
class B {
    static s = 1
    constructor(p: number {}
    id: number = 2
}
let aa = new A(1, 2);
let bb = new B(1);
aa = bb;
bb = aa;
```

```typescript
/**
  * 类中含有私有成员，互相不兼容
  */

class A {
    constructor(p: number, q: number) {}
    id: number = 1
    private name: string = ''
}
class B {
    static s = 1
    constructor(p: number {}
    id: number = 2
    private name: string = ''
}
let aa = new A(1, 2);
let bb = new B(1);
// aa = bb; // Error
// bb = aa; // Error

class C extends A {}
let cc = new C(1, 2);
aa = cc;
cc = aa;
```

### 5.2.6 泛型兼容性

```typescript
// 泛型接口
interface Empty<T> {
  value: T;
}
let obj1: Empty<number> = {};
let obj2: Empty<string> = {};
// obj1 = obj2; // Error

// 泛型函数
let log1 = <T>(x: T): T => {
  console.log("x");
  return x;
};
let log2 = <U>(y: U): U => {
  console.log("y");
  return y;
};
log1 = log2;
```

## 5.3 类型保护机制

TypeScript 能够在特定的区块中保证变量属于某种确定的类型。
可以在此区块中放心地引用此类型的属性，或调用此类型的方法。

```typescript
enum Type { Strong, Week }

 class Java {
    helloJava() {
        console.log('hello java');
    }
    java: any
 }

  class JavaScript {
    helloJavaScript() {
        console.log('hello JavaScript');
    }
    javascript: any
 }

 // 4. 类型保护函数
 // 返回值类型 => “lang is Java” 叫做：类型谓词
 function isJava(lang: Java | JavaScript): lang is Java {
    return (lang as Java).helloJava !== undefined)
 }

 function getLanguage(type: Type){
    let lang = type === Type.Strong ? new Java() : new JavaScript();
    // 此处无法满足需要
    // if (lang.helloJava) {
    //    lang.helloJava();
    // } else (lang.helloJavaScript) {
    //    lang.helloJavaScript();
    // }


    // 制造一个区块
    // 1. instanceof 关键字
    if (lang instanceof Java) {
        lang.helloJava(); // OK
    } else {
        lang.helloJavaScript(); // OK
    }

    // 2. in 关键字
    if ('java' in lang) {
        lang.helloJava(); // OK
    } else {
        lang.helloJavaScript(); // OK
    }

    // 3. typeof 关键字
    if (typeof x === 'string'){
        x.length; // OK
    } else {
        x.toFixed(2); // OK
    }

    // 4. 类型保护函数
    if (isJava(lang)) {
        lang.helloJava(); // OK
    } else {
        lang.helloJavaScript(); // OK
    }

    return lang
 }

 getLanguage(Type.Strong);
```

# 六、高级类型

---

## 6.1 交叉类型与联合类型

交叉类型：适合做对象混入
联合类型：类型具有不确定性，增强代码的灵活性

```typescript
interface DogInterface {
  run(): void;
}
interface CatInterface {
  jump(): void;
}
let pet: DogInterface & CatInterface = {
  run() {},
  jump() {},
};
```

```typescript
let a: number | string = 1; // OK
let a: number | string = "a"; // OK

// 取值只能在 a,b,c 中
let b: "a" | "b" | "c" = "a"; // OK
```

```typescript
class Dog implements DogInterface {
  run() {}
  eat() {}
}

class Cat implements CatInterface {
  jump() {}
  eat() {}
}

enum Master {
  Boy,
  Girl,
}
function getPet(master: Master) {
  let pet = master === Master.Boy ? new Dog() : new Cat();
  pet.eat();
  // pet.run(); // Error
  // pet.jump(); // Error
  return pet;
}
```

```typescript
interface Square {
  kind: "square";
  size: number;
}
interface Rectangle {
  kind: "rectangle";
  width: number;
  height: number;
}
interface Circle {
  kind: "circle";
  r: number;
}

type Shape = Square | Rectangle | Circle;
function area(s: Shape) {
  switch (s.kind) {
    case "square":
      return s.size * s.size;
    case "rectangle":
      return s.width * s.height;
    case "circle":
      return Math.PI * s.r ** 2;
    default:
      return ((e: never) => {
        throw new Error(e);
      })(e);
  }
}
```

## 6.2 索引类型

```typescript
let obj = {
  a: 1,
  b: 2,
  c: 3,
};

function getValues(obj: value, keys: string[]) {
  return keys.map((key) => obj[key]);
}

console.log(getValues(obj, ["a", "b"])); // 1, 2
console.log(getValues(obj, ["e", "f"])); // undefined, undefined
```

```typescript
// 如何约束数组的元素
// 下面来改造以下函数

function getValues<T, K extends keyof T>(obj: T, keys: K[]): T[K][] {
  return keys.map((key) => obj[key]);
}

console.log(getValues(obj, ["a", "b"])); // OK
// console.log(getValues(obj, ['e', 'f'])); // Error
```

## 6.3 映射类型

```typescript
interface Obj {
  a: string;
  b: number;
  c: boolean;
}

type ReadonlyObj = Readonly<Obj>;

type PartialObj = Partial<Obj>;

type PickObj = Pick<Obj, "a" | "b">;

type RecordObj = Record<"x" | "y", Obj>;
```

## 6.4 条件类型

```typescript
// T extends U ? X : Y
type TypeName<T> = T extends string
  ? "string"
  : T extends number
  ? "number"
  : T extends boolean
  ? "boolean"
  : T extends undefined
  ? "undefined"
  : T extends Function
  ? "function"
  : "object";

type T1 = TypeName<string>; // T1类型是 string
type T2 = TypeName<string[]>; // T1类型是 object

// ( A | B ) extends U ? X : Y
// ( A extends U ? X : Y ) | ( B extends U ? X : Y )
type T3 = TypeName<string | string[]>;

type Diff<T, U> = T extends U ? never : T;

type T4 = Diff<"a" | "b" | "c", "a" | "e">;

type NotNull<T> = Diff<T, undefined | null>;
type T5 = NotNull<string | number | undefined | null>;

// 官方预置的：
// Exclude<T, U>
// NonNullable<T>
// Extract<T, U>

type Y7 = ReturnType<() => stringn>;
```

# 七、ES6 与 CommonJS 的模块系统

---

## 7.1 两种模式不要混用

1. ES6
2. CommonJS

# 八、命名空间

---

```typescript
// a.ts
namespace Shape {
  const pi = Math.PI;
  export function cricle(r: number) {
    return pi * r ** 2;
  }
}

// b.ts
/**
 * 下面是三斜线指令
 * 在 b.ts 调用 a.ts 的 cricle 方法 必须加上此指令
 */
/// <reference path="a.ts" />

namespace Shape {
  export function square(x: number) {
    return x * x;
  }
}

console.log(Shape.cricle(2)); // 12.56 共享命名空间
console.log(Shape.pi); // underfined 命名空间内的属性必须导出才能访问
console.log(Shape.square(2)); // 4  共享命名空间

/**
 * 为了使得写法更加简便
 * 可以使用此写法
 * 此写法和模块没有关系
 */
import cricle = Shape.cricle;
console.log(cricle(1)); // 3.14
```

# 九、声明合并

---

## 9.1 接口合并

```typescript
/**
 * 后申明的接口顺序靠前，接口内部函数先声明的靠前
 * 函数参数类型为字面量时，优先级最高
 */

interface A {
  x: number;
  // y: string; // Error
  foo(bar: number): number; // 函数重载顺序 5
  foo(bar: "a"): number; // 函数重载顺序 2
}
interface A {
  y: number;
  foo(bar: string): string; // 函数重载顺序 3
  foo(bar: number[]): number[]; // 函数重载顺序 4
  foo(bar: "b"): number; // 函数重载顺序 1
}

// 这个变量必须具备这两个接口的所有成员
let a: A = {
  x: 1,
  y: 1,
  foo(bar: any) {
    // 函数重载
    return bar;
  },
};
```

## 9.2 函数与命名空间合并

```typescript
function Lib() {}
namespace Lib {
  export let version = "1.0";
}
console.log(Lib.version); // 1.0
```

## 9.3 类与命名空间合并

```typescript
class C {}
namespace C {
  export let state = "1";
}
console.log(C.state); // 1
```

## 9.4 枚举与命名空间合并

```typescript
enum Color {
  Red,
  Yellow,
  Bule,
}
namespace Color {
  export function mix() {}
}
console.log(Color);
/**
 * {
 *     0: "Red",
 *     1: "Yellow",
 *     2: "Bule",
 *     "Red": 0,
 *     "Yellow": 1,
 *     "Blue": 2,
 *     "mix": f mix ()
 * }
 */
```

# 十、如何编写声明文件

---

引入第三方类库并为它们编写声明文件
类库分为三种：

1. 全局类库
2. 模块类库
3. umd 类库

# 十一、引入 jQuery

---

## 11.1 安装

```cmd
npm i jquery
npm i @types/jquery -D
```

大多数类库都会提供声明文件
可以上 [TypeSearch](microsoft.github.io/TypeSearch) 去查找社区有没有为类库提供声明文件

## 11.2 使用

```typescript
import $ from "jquery";
$(".app").css("color", "red");
```

# 十二、如何自己编写一个声明文件

---

> 可以参考此网站 [Definitely Typed](definitelytyped.org/guides/contributing.html)

## 12.1 全局库

```typescript
// global-lib.js
function globalLib(options) {
  console.log(options);
}

globalLib.version = "1.0.0";

globalLib.doSomething = function () {
  console.log("global Lib do something");
};

// global-lib.d.js 声明文件
declare function globalLib(options: globalLib.Options): void;

declare namespace globalLib {
  const version: string;
  function doSomething(): void;
  interface Options {
    [key: string]: any;
  }
}

// example.ts 使用示范
console.log(globalLib({ x: 1 }));
console.log(globalLib.doSomething());
```

## 12.2 模块库

```typescript
// module-lib.js
function moduleLib(options) {
  console.log(options);
}

moduleLib.version = "1.0.0";

moduleLib.doSomething = function () {
  console.log("moduleLib Lib do something");
};

module.exports = moduleLib;

// module-lib.d.js 声明文件
declare function moduleLib(options: globalLib.Options): void;

interface Options {
  [key: string]: any;
}

declare namespace moduleLib {
  const version: string;
  function doSomething(): void;
}

export = moduleLib;

// example.ts 使用示范
import moduleLib from "./module-lib.js";

console.log(moduleLib.doSomething());
```

## 12.3 umd 库

##### tsconfig 默认不允许 umd 使用全局引用方式，若如果想通过全局引用，则需要打开设置 "allowUmdGlobalAccess": true

```typescript
// umd-lib.js
(function (root, factory) {
  if (typeof define === "function" && define.amd) {
    define(factory);
  } else if (typeof module === "object" && module.exports) {
    module.exports = factory();
  } else {
    root.umdLib = factory();
  }
})(this, function () {
  return {
    version: "1.0.0",
    doSomething() {
      console.log("umdLib do something");
    },
  };
});

// umd-lib.d.js 声明文件
declare namespace umdLib {
  const version: string;
  function doSomething(): void;
}

export as namespace umdLib;
export = umdLib;

// example.ts 使用示范
import umdLib from "./umd-lib.js";

console.log(umdLib.doSomething());
```

## 12.4 给类库添加自定义方法

> 这里以 moment 为例

```typescript
import m from "moment";

declare module "moment" {
  export function myFunction(): void;
}

m.myFunction = () => {};
```

## 12.5 声明文件的依赖

> 这里以 jQuery 为例

###### /node_modules/@types/jquery/package.json

```json
"types": "index"
```

###### /node_modules/@types/jquery/index.d.js

```typescript
/// <reference types="sizzle" />
/// <reference types="jQueryStatic.d.ts" />
/// <reference types="JQuery.d.ts" />
/// <reference types="misc.d.ts" />
/// <reference types="legacy.d.ts" />

export = jQuery;
```
