---
title: TypeScript 项目使用 Eslint + Prettier + husky 提高前端项目质量
author: Oliver
top: false
cover: true
toc: false
mathjax: false
date: 2020-11-07 9:24:45
img: /images/1_83PZeBAFQkP1XyOfDigxsg.png
password:
summary: 如何让团队拥有更好的代码意识？如何使团队的代码风格近乎统一？如何防止错误的、不严谨的代码被提交？
tags:
  - Git
  - ESLint
  - Prettier
  - Husky
  - TypeScript
categories:
  - 前端
keywords: typescript, ts, git, githook, hook, eslint, prettier, husky, 代码质量, 代码风格, 前端规范, npm, node, git提交, git提交检查, 代码检查, git提交前检查, git提交前编译, 代码风格统一, 代码美化, 代码格式化, 前端工程化
---

# 前言

最近几年 TypeScript 越来越得到开发者的喜爱，所以在我的推动下，公司也开始使用 TypeScript 来开发微信小程序（正好微信开发工具也支持了）。选择它的原因是因为 TypeScript 提供了静态类型检查的功能，而且还可以使用一些面向对象的编程语法，使得前端代码更加灵活更加严谨。

# 规范问题

由于使用 TypeScript 作为开发微信小程序的语言，所以做了一些小调整，在 Git 提交时排除了所有后缀为 .js 的文件。这样做的好处是，减少冗余文件的提交，只需要提交 .ts 的文件即可。其他开发人员只能修改 .ts 文件，编译出的 .js 只作为本地预览开发使用。但是最终提交到腾讯服务器的还是 .js 文件而不是 .ts。

所以为了防止一些同事，忘记上传 .ts 文件导致的问题（没有 .ts 文件就无法编译出 .js，没有 .js 文件小程序就无法执行）、还有每个人代码风格不一致，或者代码不规范不严谨，因此我产生几个问题：

- 如何让团队拥有更好的代码意识？
- 如何使团队的代码风格近乎统一？
- 如何防止错误的、不严谨的代码被提交？

# 工具介绍

- [ESLint](https://eslint.org/docs/user-guide/getting-started) - 代码检查工具
- [Prettier](https://prettier.io/docs/en/install.html) - 代码格式化工具
- [Husky](https://github.com/typicode/husky) - Git Hook（Git 钩子）

# 使用教程

## 安装依赖

```bash
npm i -D @typescript-eslint/eslint-plugin @typescript-eslint/parser eslint eslint-config-prettier eslint-plugin-prettier eslint-plugin-typescript husky prettier
```

## 配置 .eslintrc.js

```js
module.exports = {
  root: true,
  env: {
    node: true,
    browser: true,
    es6: true,
  },
  extends: [
    "plugin:@typescript-eslint/recommended",
    "prettier/@typescript-eslint", // 样式规范以 prettier 为准
    "plugin:prettier/recommended", // 样式规范以 prettier 为准
  ],
  rules: {
    // 自定义规则
    "prettier/prettier": 1,
    "no-console": 0,
    eqeqeq: ["warn", "always"],
    "prefer-const": [
      "error",
      { destructuring: "all", ignoreReadBeforeAssign: true },
    ],
    "@typescript-eslint/ban-types": 0,
    "@typescript-eslint/no-explicit-any": 0,
    "@typescript-eslint/explicit-module-boundary-types": 0,
    "@typescript-eslint/no-unused-vars": 0,
    "@typescript-eslint/interface-name-prefix": 0,
    "@typescript-eslint/explicit-member-accessibility": 0,
    "@typescript-eslint/no-triple-slash-reference": 0,
    "@typescript-eslint/ban-ts-ignore": 0,
    "@typescript-eslint/no-this-alias": 0,
    "@typescript-eslint/triple-slash-reference": [
      "error",
      { path: "always", types: "never", lib: "never" },
    ],
  },
  parserOptions: {
    parser: "@typescript-eslint/parser",
  },
};
```

## 配置 .prettierrc

具体配置参考官网：[Options](https://prettier.io/docs/en/options.html)

```json
{
  "printWidth": 80,
  "tabWidth": 2,
  "singleQuote": true,
  "semi": true,
  "trailingComma": "none",
  "endOfLine": "auto",
  "bracketSpacing": true,
  "jsxBracketSameLine": true,
  "arrowParens": "always",
  "parser": "typescript",
  "eslintIntegration": true
}
```

## 配置 package.json

添加 husky 配置，添加 eslint 命令

```json
{
  "scripts": {
    "eslint": "eslint . --ext .ts  --fix",
    "compile": "./node_modules/typescript/bin/tsc",
    "tsc": "node ./node_modules/typescript/lib/tsc.js"
  },
  "husky": {
    "hooks": {
      "pre-commit": "npm run eslint && npm run tsc"
    }
  }
}
```

# 测试

在提交代码前，ESLint 会根据代码规约检查代码，Prettier 会根据规则自动格式化代码，在这之后会编译 .ts 文件。如果有任何不符合规范的代码，或者是编译报错，提交将无法完成。

```bash
git add *
git commit -m 'dev'
```

# 小结

借助工具能够实现半自动化代码检查、格式化、编译。
