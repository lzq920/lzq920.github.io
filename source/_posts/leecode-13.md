---
title: 整数反转【LeeCode】
date: 2021-12-19 15:30:54
tags:
  - LeetCode
---

给你一个 32 位的有符号整数 x ，返回将 x 中的数字部分反转后的结果。

如果反转后整数超过 32 位的有符号整数的范围 [−231, 231 − 1] ，就返回 0。

假设环境不允许存储 64 位整数（有符号或无符号）。

```js
var reverse = function (x) {
  let res = 0;
  let flag = 1;
  if (x < 0) {
    flag = -1;
    x = -x;
  }
  while (x) {
    res = res * 10 + (x % 10);
    x = Math.floor(x / 10);
  }
  if (res > Math.pow(2, 31) - 1) {
    return 0;
  }
  return res * flag;
};
```
