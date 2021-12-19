---
title: 最长公共前缀 【LeetCode】
date: 2021-12-19 15:57:24
tags:
  - LeetCode
---

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

```js
var longestCommonPrefix = function (strs) {
  if (strs.length == 0) return "";
  var prefix = strs[0];
  for (var i = 1; i < strs.length; i++) {
    while (strs[i].indexOf(prefix) != 0) {
      prefix = prefix.substring(0, prefix.length - 1);
      if (prefix == "") return "";
    }
  }
  return prefix;
};
```
