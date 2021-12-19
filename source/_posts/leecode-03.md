---
title: 旋转数组【LeeCode】
date: 2021-12-19 14:24:14
tags:
  - LeeCode
---

给你一个数组，将数组中的元素向右轮转 k 个位置，其中 k 是非负数。

```js
var rotate = function (nums, k) {
  k = k % nums.length;
  nums.unshift(...nums.splice(nums.length - k, k));
  return nums;
};
```
