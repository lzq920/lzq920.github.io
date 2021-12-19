---
title: 存在重复元素【LeeCode】
date: 2021-12-19 14:25:53
tags:
  - LeeCode
---

给定一个整数数组，判断是否存在重复元素。

如果存在一值在数组中出现至少两次，函数返回 true 。如果数组中每个元素都不相同，则返回 false 。

```js
var containsDuplicate = function (nums) {
  return new Set(nums).size !== nums.length;
};
```
