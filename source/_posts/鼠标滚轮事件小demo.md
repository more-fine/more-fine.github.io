---
title: 鼠标滚轮事件小demo
date: 2020-09-29 23:12:46
tags: [小栗子]
categories: [小栗子]
---

## [鼠标滚轮事件小demo](https://www.cnblogs.com/MrXXD/p/13298188.html)

一般很多网站都会有一些这样的效果来提升用户的体验度,返回顶部的功能啊,当然也包括一些顶部的菜单滑动显示隐藏的功能等

也是记录一下以便以后方便拿来用,有需要的也给您提供一份便捷.可能写的不太好,欢迎交流探讨

 

### 简单的举个栗子:

上滑效果:

![img](/img/2021/1809041-20200714110315176-1377970511.png)

上滑效果:

![img](/img/2021/1809041-20200714110321236-684601970.jpg)

返回顶部效果:

![img](/img/2021/1809041-20200714110222524-536348246.png)

 

### 记录一些效果:

1,简单的动画实现返回顶部,或者你想要去到的位置



```js
    // 页面返回响应位置
    $(".fixedmodular1").click(function () {
        // 返回.main元素的位置
        let target_top = $(".main").offset().top;
                //设置动画效果,不会中断滚动
        $("html,body").animate({ scrollTop: target_top }, 1000);
                //返回顶部
               //$("html,body").animate({ scrollTop: 0}, 1000);
    })        
```



感觉这样确实不是很友好,很简单粗暴.

2,返回顶部的图标,显示隐藏,包括顶部的样式也是同样的套路



```js
$(window).scroll(function() {
    var scrolltop = $(this).scrollTop(); //滚动条
    var windowheight = $(this).height(); //可视化窗口高度
    if (scrolltop >= windowheight) {
        $('#top').show()
    } else {
        $('#top').hide()
    }
})
```



3,返回顶部的操作



```js
var timer = null
$('#top').click(function() {
    timer = setInterval(function() {
        var osTop = document.documentElement.scrollTop||document.body.scrollTop;
        var ispeed = Math.floor(-osTop / 6);
        //获取滚动条距离顶部的高度
        document.documentElement.scrollTop = document.body.scrollTop = osTop + ispeed;
        if (osTop == 0) {
            clearInterval(timer);
        }
    }, 30);
})
//在滚动的过程中滑动鼠标中止滚动,停到当前滚动的位置

window.addEventListener('mousewheel', mouseScrollMove,false)



function mouseScrollMove(e) {

   var eM = e.wheelDelta || e.detail;

   var moveLen = Math.max(-1, Math.min(1, eM));

   if (moveLen < 0) { //向下滚动

       clearInterval(timer);

   } else { //向上滚动

       clearInterval(timer);

   }

}
```



 