---
title: select标签在iOS上兼容性问题
date: 2021-08-21 14:00
---
### select标签在iOS上兼容性问题

在部分iOS版本上，select标签第一个在没有切换的情况下，直接点击完成会选不中

解决方案:增加一个不可选中的默认项

```
<select>
    <option value="" disabled>请选择</option>
    <option value="1">男</option>
    <option value="2">女<option>
</select>
```
