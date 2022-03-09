---
title: angular中使用ng-repeat报错
date: 2021-07-30 21:53:31
tags: [备忘录,angular]
categories: [备忘录]
---

### angular中使用ng-repeat报错

**Angular项目 Error: [ngRepeat:dupes\] Duplicates in a repeater are not allowed.报错**



在angular的项目里，一不小心就会出现这个错误[ngRepeat:dupes] ，这个问题是因为内容有重复引起的解决起来挺简单

在对应的ng-repeat指令中增加track by $index，意思是用索引值识别

例：  

```
<p ng-repeat="item in ages track by $index">
　　{{item}}
</p>
```

![angular使用ngRepeat报错](/img/2021/angular报错.png)

