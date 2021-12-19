---
title: 字符串中的第一个唯一字符【LeeCode】
date: 2021-12-19 15:34:16
tags:
  - LeetCode
---

给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。

```js
var firstUniqChar = function (s) {
  var hash = {};
  for (var i = 0; i < s.length; i++) {
    if (hash[s[i]]) {
      hash[s[i]]++;
    } else {
      hash[s[i]] = 1;
    }
  }
  for (var i = 0; i < s.length; i++) {
    if (hash[s[i]] == 1) {
      return i;
    }
  }
  return -1;
};
```
