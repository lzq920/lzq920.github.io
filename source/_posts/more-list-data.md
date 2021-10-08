---
title: 解决超10000+数据大列表渲染问题
---

### 解决超10000+数据大列表渲染问题

```
<ul class="list"></ul>
```

```
const total = 10000,
  // 每次处理20条
  each = 20,
  // 需要处理次数
  needTimes = Math.ceil(total / each),
  content = document.querySelector('.list')
  // 当前处理次数
  let currentTime = 0
  function add() {
     // 创建一个虚拟的节点对象
     const fragment = document.createDocumentFragment()
     // 分批处理
     for (let i = 0; i < each; i++) {
       const li = document.createElement('li')
       li.innerText = Math.floor(i+currentTime*each)
       fragment.appendChild(li)
     }
     //把虚拟的节点对象添加到主节点中
     content.appendChild(fragment)
     currentTime++;
     // 循环处理
     if (currentTime < needTimes) window.requestAnimationFrame(add);
  }
  window.requestAnimationFrame(add)


```


