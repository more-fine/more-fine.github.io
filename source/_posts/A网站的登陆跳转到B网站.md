---
title: A网站的登陆跳转到B网站
date: 2020-09-29 23:07:47
tags: [bug]
---

## A网站的登陆跳转到B网站

###   业务需求：

a网站的登陆地方输入用户名密码，直接跳转到b网站的首页，当时公司b网站已经开发好了的，直接拿了他的登陆地址url，看了network里面的login请求需要的参数，找到登陆的页面url，直接让跳转，搞了半天才解决这个问题。

 

于是就在A页面写了ajax请求，拿到测试账号之后，获取数据，可以获取到数据，然后就写跳转的页面，能跳转过去，但是存不了这个登陆的状态

![img](/img/2021/1809041-20200712182312184-1769366477.png)![img](/img/2021/1809041-20200712181934300-195766155.png)

 

 

cookie的存储状态拿到了，但是存不了，最后看来别人的分享之后直接一行代码解决，后知后觉才知道ajax请求是有同源策略的

一个刚进入开发的小博主分享，坚持遇见更好的自己！欢迎各位指点，评论，技术探讨。

## 问题解决：

完整的无歧义的表述应该是这样：
1.ajax会自动带上同源的cookie，不会带上不同源的cookie
\2. 可以通过前端设置withCredentials为true， 后端设置Header的方式让ajax自动带上不同源的cookie，但是这个属性对同源请求没有任何影响。会被自动忽略。
\3. 这是MDN对withCredentials的解释： MDN-withCredentials ，我接着解释一下同源。
众所周知，ajax请求是有同源策略的，虽然可以应用CORS等手段来实现跨域，但是这并不是说这样就是“同源”了。ajax在请求时就会因为这个同源的问题而决定是否带上cookie
————————————————
原文链接：https://blog.csdn.net/Alice_124/article/details/81705099

```js
$(".zhangButton").click(function () {
        let zhangName = $("input[name='zhangName']").val()
        let zhangPass = $("input[name='zhangPass']").val()
        let zhangUrl = 'https://xxxxxxxx/login'//登录接口
        layui.use('layer', function () {
            var layer = layui.layer;
　　　　　　　　$.support.cors = true;//允许ajax跨域
            $.ajax({
                method: 'post',
                url: zhangUrl,
                data: JSON.stringify({ loginName: zhangName, password: hex_md5(zhangPass) }),
                contentType: "application/json",//传递数据格式
                xhrFields: { withCredentials: true },//允许携带cookie
                success: function (res) {
                    console.log(res)
                    if (res.code == "200") {
                        layer.msg('正在跳转请稍等...');
                        setTimeout(function () {
                            window.open('https://xxxxxx/home')
                            }
                        }, 1000)

                    } else {
                        layer.msg('请输入正确的用户名和密码');
                    }
                }
            });
        });
    })
```

