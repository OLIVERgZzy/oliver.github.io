---
title: 我的LeetCode之旅「两数之和」
author: Oliver
top: false
cover: false
toc: false
mathjax: false
date: 2020-07-02 10:08:05
img: /images/iuLe93nYpIaKVvN.jpg
password:
summary:
tags: LeetCode
categories: 算法
keywords: 算法,javascript,leetcode,两数之和,leetcode 两数之和
---

# 一、题目描述

给定一个整数数组 nums  和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

# 二、示例

> 给定 nums = [2, 7, 11, 15], target = 9
>
> 因为 nums[0] + nums[1] = 2 + 7 = 9
> 所以返回 [0, 1]

# 三、解题

语言：javascript

思路：遍历 `nums`，用 `target` 数值与数组 `nums` 中每个数值相减获得到另一个数值，如果这个数值刚好存在`nums` 中，则返回。

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
const twoSum = function (nums, target) {
  // 遍历 `nums` 数组
  for (let i = 0; i < nums.length; i++) {
    // 求目标减去当前索引的数值
    let disc = target - nums[i];
    // 查找 `nums` 数组中是否存在这个值
    let idx = nums.indexOf(disc);
    if (idx !== i && idx >= 0) {
      return [i, nums.indexOf(disc)];
    }
  }
};
```
