---
title: 字符串转换整数 (atoi) 【LeetCode】
date: 2021-12-19 15:49:18
tags:
  - LeetCode
---

请你来实现一个 atoi 函数，使其能将字符串转换成整数。

```js
var myAtoi = function (str) {
  str = str.trim();
  if (!/^[+|-]?\d+/.test(str)) return 0;
  let val = parseInt(str.match(/^[+|-]?\d+/));
  let base = Math.pow(2, 31);
  let min = -base;
  let max = base - 1;
  return Math.max(Math.min(val, max), min);
};
```
