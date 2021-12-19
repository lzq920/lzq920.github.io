---
title: 两个数组的交集 II【LeeCode】
date: 2021-12-19 14:41:26
tags:
  - LeeCode
---

给你两个整数数组 nums1 和 nums2 ，请你以数组形式返回两数组的交集。返回结果中每个元素出现的次数，应与元素在两个数组中都出现的次数一致（如果出现次数不一致，则考虑取较小值）。可以不考虑输出结果的顺序。

```js
var intersect = function (nums1, nums2) {
  var result = [];
  var map = {};
  for (var i = 0; i < nums1.length; i++) {
    if (map[nums1[i]]) {
      map[nums1[i]]++;
    } else {
      map[nums1[i]] = 1;
    }
  }
  for (var i = 0; i < nums2.length; i++) {
    if (map[nums2[i]]) {
      result.push(nums2[i]);
      map[nums2[i]]--;
    }
  }
  return result;
};
```
