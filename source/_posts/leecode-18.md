---
title: 实现 strStr() 【LeetCode】
date: 2021-12-19 15:52:20
tags:
  - LeetCode
---

给你两个字符串 haystack 和 needle ，请你在 haystack 字符串中找出 needle 字符串出现的第一个位置（下标从 0 开始）。如果不存在，则返回 -1 。

```js
var strStr = function (haystack, needle) {
  if (needle.length == 0) return 0;
  if (haystack.length == 0) return -1;
  if (haystack.length < needle.length) return -1;
  var i = 0;
  while (i <= haystack.length - needle.length) {
    if (haystack[i] == needle[0]) {
      if (haystack.substring(i, i + needle.length) == needle) {
        return i;
      }
    }
    i++;
  }
  return -1;
};
```
