---
title: 'css多行文本溢出隐藏,显示省略号'
date: 2020-10-06 12:49:04
tags: [备忘录]
---

### css多行文本溢出隐藏-显示省略号

单行文本溢出隐藏：

overflow: hidden;
单行文本溢出隐藏显示省略号：

white-space: nowrap;
overflow: hidden;
text-overflow: ellipsis;
多行文本溢出隐藏显示省略号：

text-overflow: -o-ellipsis-lastline;
overflow: hidden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-line-clamp: 2;
-webkit-box-orient: vertical;
多行文本溢出隐藏时，-webkit-line-clamp: 2; 把2改成几就显示几行。灵活多变。
————————————————
版权声明：本文为CSDN博主「斌道天下」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/vipbin520/article/details/81483955

