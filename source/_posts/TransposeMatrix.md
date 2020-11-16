---
title: 我的LeetCode之旅「867. 转置矩阵」
author: Oliver
top: false
cover: false
toc: false
mathjax: false
date: 2020-11-16 11:21:05
img: /images/iuLe93nYpIaKVvN.jpg
password:
summary:
tags: LeetCode
categories: 算法
keywords: 算法, typescript, leetcode, 转置矩阵, leetcode 转置矩阵, Transpose Matrix
---

# 一、题目描述

给定一个矩阵 A，返回 A 的转置矩阵。
矩阵的转置是指将矩阵的主对角线翻转，交换矩阵的行索引与列索引。

# 二、示例

示例 1:

```bash
输入: [[1,2,3],[4,5,6],[7,8,9]]
输出: [[1,4,7],[2,5,8],[3,6,9]]
```

示例 2:

```bash
输入: [[1,2,3],[4,5,6]]
输出: [[1,4],[2,5],[3,6]]
```

提示:

- `1 <= A.length <= 1000`
- `1 <= A[0].length <= 1000`

# 三、解题

语言：TypeScript

思路： 选取矩阵中第一行数组进行循环，获取到当前循环的索引的同时，再利用数组的 `map` 方法循环从矩阵中每一行用索引获取值，并组装拼接成数组返回。

```js
function transpose(A: number[][]): number[][] {
  let res: number[][] = [];
  for (let rIndex = 0; rIndex < A[0].length; rIndex++) {
    res[rIndex] = A.map((row) => row[rIndex]);
  }
  return res;
}
```
