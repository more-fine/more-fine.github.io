---
title: moment.js常用操作-备忘
date: 2023-03-08 16:37:00
tags: [备忘录]

---

## moment.js常用操作-备忘

```js
// 官网  http://momentjs.cn/


// moment格式时间
moment()//此刻时间
moment("1995-12-25")
moment("1995-12-25","YYYY-MM-DD") 

// 设置moment
moment().set({'year': 2013, 'month': 3});

// 一天前
moment().subtract(1, 'day').format('YYYY-MM-DD')
// 一月前
moment().subtract(1, 'month').format('YYYY-MM-DD')
// 一年前
moment().subtract(1, 'year').format('YYYY-MM-DD')
// 一天后/month/year
moment("1995-12-25").add(1,"days").format("YYYY-MM-DD");

// 是否在某日期前
moment('2010-10-20').isBefore('2010-10-21'); // true
// 是否相等
isSame()
// 是否在某日期之后
isAfter()
```

