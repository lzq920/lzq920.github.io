---
title: 树数据转化为列表数据
---

```
function treeToList(tree, childrenName = 'children') {
  const isPlainObject = (val) =>
    !!val && typeof val === 'object' && val.constructor === Object;
  if (Array.isArray(tree)) {
    const tempList = tree.slice(0);
    const res = [];
    for (let item of tempList) {
      res.push({
        ...item,
        [childrenName]: [],
      });
      if (item[childrenName] && item[childrenName].length) {
        tempList.push(...item[childrenName].map((item) => item));
      }
    }
    return res.map((item) => {
      delete item[childrenName];
      return item;
    });
  } else if (isPlainObject(tree)) {
    const res = [];
    const deep = (obj) => {
      res.push({
        ...obj,
        [childrenName]: [],
      });
      if (isPlainObject(obj[childrenName])) {
        deep(obj[childrenName]);
      }
    };
    deep(tree);
    return res.map((item) => {
      delete item[childrenName];
      return item;
    });
  }
}
const tree = [
  {
    id: 1,
    name: '1',
    childrens: [
      {
        id: 2,
        name: '2',
        childrens: [
          {
            id: 3,
            name: '3',
          },
        ],
      },
    ],
  },
];
const tree2 = {
  id: 1,
  name: '1',
  childrens: {
    id: 2,
    name: '2',
    childrens: {
      id: 3,
      name: '3',
    },
  },
};
const result = treeToList(tree, 'childrens');
console.log(result);
const result2 = treeToList(tree2, 'childrens');
console.log(result2);
```