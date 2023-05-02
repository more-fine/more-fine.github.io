---
title: Switch使用注意
date: 2021-07-31 09:59:31
tags: [备忘录,angular]
categories: [备忘录]
---

### switch进行判断是全等的判断

先看一组写法：

```js
var str = ['1','2','1','4']
str.map((item,index)=>{
	switch (item) {
        case 1:
            str[index] = "不生效";
            break; 
        case 2:
            str[index] = "哈哈";
            break; 
        default: 
            str[index] = "期待周末~";
    } 
})
console.log(str,"str")
```

switch它是严格匹配的，所以case的值 一定要明确是字符串还是数字

switch不能这样用的，**switch是===对比的**，类型就不一样了，当然执行执行default

case '1',稍作改动就会生效,在获取到接口返回值的时候容易出错,进行switch判断,容易把类型搞错