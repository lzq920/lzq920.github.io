---
title: 列表数据转化为树状数据
date: 2021-11-03 14:00
---

```
function ListToTree(list, parentName = 'parentId', targetId = 'id') {
  const copyList = list.slice(0);
  const tree = [];
  for (let i = 0; i < copyList.length; i++) {
    for (let j = 0; j < copyList.length; j++) {
      if (copyList[i][parentName] === copyList[j][targetId]) {
        if (copyList[j].children === undefined) {
          copyList[j].children = [];
        }
        copyList[j].children.push(copyList[i]);
      }
    }
    if (copyList[i][parentName] === null) {
      tree.push(copyList[i]);
    }
  }
  return tree;
}

const List = [
  { id: 1, name: 'child1', parentId: 0 },
  { id: 2, name: 'child2', parentId: 0 },
  { id: 6, name: 'child2_1', parentId: 2 },
  { id: 0, name: 'root', parentId: null },
  { id: 5, name: 'child1_2', parentId: 1 },
  { id: 4, name: 'child1_1', parentId: 1 },
  { id: 3, name: 'child3', parentId: 0 },
  { id: 7, name: 'child3_1', parentId: 3 },
];

const tree = ListToTree(List);
console.log(tree);
```
