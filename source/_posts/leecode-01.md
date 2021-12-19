---
title: 删除排序数组中的重复项【LeeCode】
date: 2021-12-19 13:43:58
tags:
  - LeeCode
---

给你一个有序数组 nums ，请你原地删除重复出现的元素,使每个元素只出现一次，返回删除后数组的新长度。不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

```js
var removeDuplicates = function (nums) {
  if (nums.length <= 1) return nums.length;
  let i = 0;
  for (let j = 1; j < nums.length; j++) {
    if (nums[i] !== nums[j]) {
      i++;
      nums[i] = nums[j];
    }
  }
  return i + 1;
};
```
