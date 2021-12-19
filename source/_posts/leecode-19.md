---
title: 外观数列 【LeetCode】
date: 2021-12-19 15:54:18
tags:
  - LeetCode
---

给定一个正整数 n ，输出外观数列的第 n 项。

「外观数列」是一个整数序列，从数字 1 开始，序列中的每一项都是对前一项的描述。

```js
var countAndSay = function (n) {
  if (n == 1) return "1";
  var str = "1";
  for (var i = 2; i <= n; i++) {
    var count = 1;
    var temp = "";
    for (var j = 1; j < str.length; j++) {
      if (str[j] == str[j - 1]) {
        count++;
      } else {
        temp += count + str[j - 1];
        count = 1;
      }
    }
    temp += count + str[str.length - 1];
    str = temp;
  }
  return str;
};
```
