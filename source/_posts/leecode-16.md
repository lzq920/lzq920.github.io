---
title: 验证回文串 【LeetCode】
date: 2021-12-19 15:39:25
tags:
  - LeetCode
---

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

```js
var isPalindrome = function (s) {
  s = s.replace(/[^A-Za-z0-9]/g, "").toLowerCase();
  for (var i = 0, j = s.length - 1; i < j; i++, j--) {
    if (s[i] !== s[j]) {
      return false;
    }
  }
  return true;
};
```
