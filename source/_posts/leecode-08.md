---
title: 移动零【LeeCode】
date: 2021-12-19 15:00:57
tags:
  - LeeCode
---

给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

```js
var moveZeroes = function (nums) {
  let i = 0;
  let j = 0;
  while (j < nums.length) {
    if (nums[j] === 0) {
      j++;
    } else {
      nums[i] = nums[j];
      i++;
      j++;
    }
  }
  while (i < nums.length) {
    nums[i] = 0;
    i++;
  }
  return nums;
};
```
