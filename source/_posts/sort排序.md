---
title: sort排序
date: 2022-03-31 23:33:39
tags: [sort排序]
---

### sort排序

简单数组排序:

```js
var arr1 = [10,1,5,2,3];
arr1.sort(function(a, b) {
    return a - b;
});
console.log(arr1);//升序(b-a降序)
```

对象数组排序:

```js
var arr = [{id:1,age:43},{id:2,age:56},{id:3,age:6},{id:4,age:3}]

function sortby(prop, rev = true) {
     // prop 属性名
     // rev  升序降序 默认升序
      return function(a, b) {
         var val1 = a[prop]; 
         var val2 = b[prop]; 
         return rev ? val1 - val2 : val2 - val1;
      }
}

arr.sort(sortby('age')); // 根据age进行升序排列
arr.sort(sortby('age',false)); // 根据age降序
```

```js
var items = [
  { name: 'Edward', age: 21 },
  { name: 'Sharpe', age: 37 },
  { name: 'And', age: 45 },
  { name: 'The', age: -12 },
  { name: 'Magnetic',age:0 },
  { name: 'Zeros', age: 37 }
];
 //升序
items.sort(function (a, b) {
  if (a.age > b.age) {
    return 1;
  }
  if (a.age< b.age) {
    return -1;
  }
  // a 必须等于 b
  return 0;
});
```

