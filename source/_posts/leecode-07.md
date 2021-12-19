---
title: 加一【LeeCode】
date: 2021-12-19 14:50:21
tags:
  - LeeCode
---

给定一个由 整数 组成的 非空 数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

```js
var plusOne = function (digits) {
  let len = digits.length;
  let i = len - 1;
  while (i >= 0) {
    if (digits[i] === 9) {
      digits[i] = 0;
      i--;
    } else {
      digits[i]++;
      break;
    }
  }
  if (i < 0) {
    digits.unshift(1);
  }
  return digits;
};
```
