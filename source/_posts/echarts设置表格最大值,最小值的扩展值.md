---
title: echarts设置表格最大值,最小值的扩展值
date: 2021-07-32 10:27:39
tags: [备忘录,echarts]
categories: [备忘录]
---

### Echarts设置表格最大值,最小值的扩展值

#### **先看需求:**

**图1:**

![](/img/2021/echarts2.png)

**图2:**

![](/img/2021/echarts1.png)

**需求:**

将图1改成图2的形式展示,不展示负值

**代码(及解决方案):**

![](/img/2021/echarts3.png)

**解决方案入下:**

设置boundaryGap属性['0','0']或百分比,和threshold属性有冲突,最终结果去掉此属性,添加boundaryGap属性成功解决问题.

**帮助链接:**

https://blog.csdn.net/qq_44687755/article/details/97938265