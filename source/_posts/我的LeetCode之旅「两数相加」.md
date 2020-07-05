---
title: 我的LeetCode之旅「两数相加」
author: Oliver
top: false
cover: false
toc: false
mathjax: false
date: 2020-07-05 17:10:07
img: https://i.loli.net/2020/07/02/iuLe93nYpIaKVvN.jpg
password:
summary:
tags: LeetCode
categories: 算法
keywords: 算法,javascript,leetcode,两数相加,leetcode 两数相加
---

# 一、题目描述

给出两个   非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照   逆序   的方式存储的，并且它们的每个节点只能存储   一位   数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0  开头。

# 二、示例

> 输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
> 输出：7 -> 0 -> 8
> 原因：342 + 465 = 807

# 三、解题

语言：javascript

思路：循环两个链表，挨个取出其中的 val，同时组装成一个链表的数据结构。

```js
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
const addTwoNumbers = function (l1, l2) {
  let carry = false; // 是否需要进位
  let result = (next = {}); // 最终返回的结果
  // 循环，直到 l1 和 l2 同时为 null
  while (l1 || l2) {
    let v = carry ? 1 : 0; // 本轮的计算相加的变量 v（上一轮需要进位，所以可能初始值为1）
    carry = false; // 重置
    // 如果 l1 不为空，v 追加 l1.val，同时将 l1.next 覆盖当前作用域的 l1
    if (l1) (v += l1.val), (l1 = l1.next);
    // 同上
    if (l2) (v += l2.val), (l2 = l2.next);
    next.val = v % 10; // 取余数
    if (v >= 10) {
      // 因为本轮计算结果大于等于10，需要进位
      // 所以将下一个链表中值修改成 1
      next = next.next = { val: 1 };
      carry = true;
    } else {
      next = next.next = l1 || l2;
    }
  }
  return result;
};
```
