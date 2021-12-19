---
title: 有效的字母异位词 【LeetCode】
date: 2021-12-19 15:37:03
tags:
  - LeetCode
---

给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

```js
var isAnagram = function (s, t) {
  if (s.length != t.length) return false;
  let map = new Map();
  for (let i = 0; i < s.length; i++) {
    if (map.has(s[i])) {
      map.set(s[i], map.get(s[i]) + 1);
    } else {
      map.set(s[i], 1);
    }
  }
  for (let i = 0; i < t.length; i++) {
    if (map.has(t[i])) {
      if (map.get(t[i]) == 1) {
        map.delete(t[i]);
      } else {
        map.set(t[i], map.get(t[i]) - 1);
      }
    } else {
      return false;
    }
  }
  return true;
};
```
