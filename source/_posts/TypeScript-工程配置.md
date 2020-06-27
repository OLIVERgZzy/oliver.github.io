---
title: TypeScript 工程配置
author: Oliver
top: false
cover: false
toc: false
mathjax: false
date: 2020-06-26 22:16:32
img: https://i.loli.net/2020/06/26/FXahC9M3wfxZtjO.jpg
password:
summary: TypeScript 工程化配置入门
tags: TypeScript
categories: 前端
keywords: typescript,typescript中文教程,typescript工程配置
---

# 一、文件选项配置

```json
{
  "extends": "./tsconfig.base.json", // 继承其他配置文件 可以被覆盖

  "files": ["src/a.ts"], // 编译器需要编译的单个文件列表

  "include": [
    "src", // 编译器只编译src下的文件（包含子目录）
    "src/*", // 编译器只编译src一级目录下的文件
    "src/*/*" // 编译器只编译src下二级目录下的文件
  ], // 编译器需要编译的文件或目录（包含子目录）列表

  "exclude": [
    "src/lib" // 只排除src/lib所有文件
  ], // 编译器需要排除的文件或文件夹（默认排除node_module和其他声明文件）

  "compileOnSave": true // 保存时自动编译（vscode尚未支持 2020.02.03）
}
```

# 二、 编译选项配置

```json
{
  "compilerOptions": {
    "incremental": true, // 增量编译，提高编译速度
    "tsBuildInfoFile": "./buildFile", // 增量编译文件的存储位置
    "diagnostics": true, // 打印诊断信息

    "target": "es5", // 目标语言的版本
    "module": "commonjs", // 生成代码的模块标准
    "outFile": "./app.js", // 将多个互相依赖的文件生成一个文件，可以用在 AMD 模块中

    "lib": [
      "es2019.array", // 导入此类库，可以使用高版本的语法
    ], // TS 需要引用的库，即声明文件，es5 默认 "dom", "es5", "scripthost"

    "allowJs": true, // 允许编译 JS 文件 （js、jsx）
    "checkJs": true, // 允许在 JS 文件中报错，通常与 allowJs 一起使用
    "outDir": "./out",, // 指定输出目录
    "rootDir": "./", // 指定输入文件目录（用于输出）

    "declaration": true, // 生成声明文件
    "declarationDir": "./d", // 声明文件的路径
    "emitDeclarationOnly": true, // 只生成声明文件
    "sourceMap": true, // 生成目标文件的 sourceMap
    "inlineSourceMap": true, // 生成目标文件的 inline sourceMap
    "declarationMap": true, // 生成声明文件的 sourceMap

    "typeRoots": [], // 声明文件目录，默认 node_module/@types
    "types": [], // 只加载某个声明包文件列表

    "removeComments": true, // 删除注释

    "noEmit": true, // 不输出文件
    "noEmitOnError": true, // 发生错误时，不输出文件
    "noEmitHelpers": true, // 不生成 helper 函数，需额外安装 ts-helpers
    "importHelps": true, // 通过 tslib 引入 helper 函数，文件必须是模块
    "downlevelIteration": true, // 降级遍历器的实现（es3/5）

    "strict": true, // 开启所有严格的类型检查
    "alwaysStrict": false, // 在代码中主入 "use strict"
    "noImplicitAny": false, // 不允许隐式的 any 类型
    "strictNullChecks": false, // 不允许把 null、undefined 赋值给其他类型变量
    "strictPropertyInitalization": false, // 类实例属性必须 初始化
    "strictBindCallAppy": false, // 严格的 bind/call/apply 检查
    "noImplicitThis": false, // 不允许 this 有隐式的 any 类型

    "noUnuserLocals": true, // 检查只声明，未使用的局部变量
    "noUnusedParameters": true, //  检查未使用的函数参数
    "noFallthroughCasesInSwitch": true, // 防止 switch 语句贯穿
    "noImplicitReturns": true, // 每个分支都要有返回值

    "esMoudleInterop": true, // 允许 export = 导出，由 import from 导入
    "allowUmdGlobalAccess": true, // 允许在模块中放访问 UMD 全局变量

    "moduleResolution": "node", // 模块解析策略
    "baseUrl": "./", // 解析非相对模块的基地址
    "paths": {
        "jquery": ["node_modules/jquery/dist/jquery.slim.min.js"]
    }, // 为jquery指定一个精简的文件

    "rootDirs": ["src", "out"], // 将多个目录放在一个虚拟目录下，用于运行时

    "listEmittedFiles": true, // 打印输出文件
    "listFiles": true, // 打印编译的文件（包括引用的声明文件）
  }
}
```

# 三、工程引用

> 场景：在一个代码仓库中，存放多个需要单独构建的工程。
> 例如一个全栈工程，有客户端和服务端，提取出共一些公用的代码，存放到一个公共文件夹下。

```
├─src --------------------------------------------- // 项目代码
│ ├─client ---------------------------------------- // 客户端
│ │ ├─index.ts ------------------------------------ // 客户端示例文件
│ │ └─tsconfig.json ------------------------------- // 客户端配置文件
│ ├─common ---------------------------------------- // 公共
│ │ ├─index.ts ------------------------------------ // 公共示例文件
│ │ └─tsconfig.json ------------------------------- // 公共配置文件
│ ├─server ---------------------------------------- // 服务端
│ │  ├─index.ts ----------------------------------- // 服务端示例文件
│ │  └─tsconfig.json ------------------------------ // 服务端配置文件
├─test -------------------------------------------- // 测试用例
│ ├─client.test.ts -------------------------------- // 客户端测试用例
│ ├─server.test.ts -------------------------------- // 服务端测试用例
│ └─tsconfig.json --------------------------------- // 测试用例配置文件
└─tsconfig.json ------------------------------------// 整个项目的配置
```

### 1. 整个项目的配置

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "strict": true,
    "composit": true, // 工程可以被引用，可以增量编译
    "declaration": true
  }
}
```

### 2. 客户端配置文件

```json
{
  "extends": "../../tsconfig.json",
  "compilerOptions": {
    "outDir": "../../dist/client"
  },
  "references": [{ "path": "../common" }]
}
```

### 3. 服务端配置文件

```json
{
  "extends": "../../tsconfig.json",
  "compilerOptions": {
    "outDir": "../../dist/server"
  },
  "references": [{ "path": "../common" }]
}
```

### 4. 测试用例配置文件

```json
{
  "extends": "../tsconfig.json",
  "references": [{ "path": "../src/client" }, { "path": "../src/server" }]
}
```

### 5. 构建

```bash
tsc -b src/client --verbose // 构建客户端
tsc -b src/server --verbose // 构建服务端

tsc -b test --clean // 清空构建的文件
```

### 6. 工程引用优点

- 解决输出目录结构的问题
- 解决了单个工程构建的问题
- 通过增量编译提高编译速度

# 四、编译工具

### 1. 如何选择 TypeScript 编译工具？

1. 如果没有使用过 Babel，首选 TypeScript 自生的编译器（可配合 ts-loader 使用）
2. 如果项目中已经使用了 Babel，安装 @bable/preset-typescript（可配合 tsc 做类型检查）
3. 两种编译工具不要混用

### 2. ts-loader（推荐）

```js
// webpack.base.config.js
const HtmlWebpackPlugin = require("html-webpack-plugin");
const ForkTsCheckerWebpackPlugin = require("fork-ts-checker-webpack-plugin");

module.exports = {
  entry: "./src/class.ts",
  output: {
    filename: "app.js",
  },
  resolve: {
    extensions: [".js", ".ts", ".tsx"],
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/i,
        use: [
          {
            loader: "ts-loader",
            options: {
              transpileOnly: false, // 为true时只做语言转换，不做类型检查（编译时无法发现错误，需要借助第三方插件：fork-ts-checker-webpack-plugin）
            },
          },
        ],
        exclude: /node_modules/,
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/tpl/index.html",
    }),
    new ForkTsCheckerWebpackPlugin(),
  ],
};
```

### 3. awesome-typescript-loader（不推荐）

> 与 ts-loader 的主要区别：
>
> 1. 更适合与 Babel 集成，使用 Babel 的转义和缓存
> 2. 不需要安装额外的插件，就可以把类型检查放在独立进程中进行

```js
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { CheckerPlugin } = require("awesome-typescript-loader");

module.exports = {
  entry: "./src/class.ts",
  output: {
    filename: "app.js",
  },
  resolve: {
    extensions: [".js", ".ts", ".tsx"],
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/i,
        use: [
          {
            loader: "awesome-typescript-loader",
            options: {
              transpileOnly: true,
            },
          },
        ],
        exclude: /node_modules/,
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/tpl/index.html",
    }),
    new CheckerPlugin(),
  ],
};
```

### 4. Babel 7+

#### package.json

```json
{
  "name": "ts-babel-leaning",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "babel src --out-dir dist --extensions \".ts,.tsx\"",
    "type-check": "tsc --watch" // 需要单独开启一个线程，执行ts类型检查
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@babel/cli": "^7.8.4",
    "@babel/core": "^7.8.4",
    "@babel/plugin-proposal-class-properties": "^7.8.3",
    "@babel/plugin-proposal-object-rest-spread": "^7.8.3",
    "@babel/preset-env": "^7.8.4",
    "@babel/preset-typescript": "^7.8.3",
    "typescript": "^3.7.5"
  }
}
```

#### tsconfig.json

```json
{
  "compilerOptions": {
    "noEmit": true // 与 Babel 混用时，此选项请开启
  }
}
```

#### Babel 与 TypeScript 两者结合

> Babel 只做语言转换
> TypeScript 只做类型检查

#### 在 Babel 中使用 TypeScript 的注意事项

1. 命名空间在 Babel 中编译会报错，不要使用
2. 类型断言写法使用 as
3. 常量枚举，编译报错
4. 默认导出，编译报错

# 五、代码检查工具

## 1. ESLint

![ESLint.jpg](https://i.loli.net/2020/06/26/Gp2QmlDX7uWIzKV.jpg)

![ESLint.jpg](https://i.loli.net/2020/06/26/AagbK6kjqeBI9FV.jpg)

![ESLint.jpg](https://i.loli.net/2020/06/26/HfmKGColOe4rAwJ.jpg)

## 2. 如何在 TypesSript 中使用 ESLint

### package.json

需要安装这两个插件

```json
{
  "scripts": {
    "lint": "eslint src --ext .js,.ts" // 检查 .js .ts 文件
  },
  "devDependencies": {
    "@typescript-eslint/eslint-plugin": "^2.19.0",
    "@typescript-eslint/parser": "^2.19.0"
  }
}
```

### .eslintrc.json

```json
{
  "parser": "@typescript-eslint/parser",
  "plugins": ["@typescript-eslint"],
  "parserOptions": {
    "project": "./tsconfig.json"
  },
  "extends": ["plugin:@typescript-eslint/recommended"],
  "rules": {
    "@typescript-eslint/no-inferrable-types": "off" // 关闭禁止显示的声明ts类型
  }
}
```

### 3. 安装 VSCode ESLint 插件

#### setting.json

```json
{
  "eslint.autoFixOnSave": true,
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    {
      "language": "typescript",
      "autoFix": true
    },
    {
      "language": "html",
      "autoFix": true
    },
    {
      "language": "vue",
      "autoFix": true
    }
  ]
}
```

## 4. babel-eslint 与 typescript-eslint

- babel-eslint: 支持 TypeScript 没有的额外的语法检查，抛弃 TypeScript，不支持类型检查
- typescript-eslint：基于 TypeScript 的 AST，支持创建基于类型信息的规则（tsconfig.json）

**建议：**

- 两者底层机制不一样，不要一起使用
- Babel 体系建议使用 babel-eslint，否则就可以使用 typescript-eslint
