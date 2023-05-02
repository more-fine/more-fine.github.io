---
title: uni-app总结
date: 2020-09-19 23:30:45
tags:
---

# uniapp学习

### 1,创建项目

hbuilderX创建项目,输入小程序id,微信开发者工具设置安全选项,建立连接

### 2,样式不生效问题:

![1598970919039](/img/2021/1598970919039.png)

下载uniapp-hello文件,(gitHub)复制uni.css文件到自己的项目

同时在app.vue文件引入css模块

@import url("./common/uni.css");

同其他ui框架一样,会编译报错,缺少文件报错,找到static文件下面的相对应的文件,同样复制粘贴过来就OK啦,当时缺少了一个这样格式的文件---uni.ttf

### 3.h5跨域问题

在配置文件manifest.json中底部添加此内容,在其他编辑器打开,

target是跨域的地址,port是端口号,

api是指代跨域地址,前端代码直接用api代替跨域的地址

```js
"h5": {
		"devServer": {
			"port":8080,
			"disableHostCheck": true,
			"proxy": {
				"/api": {
					"target": "http://localhost:8888", 
					"changeOrigin": true,
					"secure": false,
					"pathRewrite": {
						"^/api": "" 
					}
				}
			}
        },
        "doman":"http://localhost:8888",
        "router":{
            "mode":"hash"
        }
	}
```

### 4,uniapp的条件编译

小程序,h5,支付宝等平台的差异代码渲染,[https://uniapp.dcloud.io/platform?id=%e6%9d%a1%e4%bb%b6%e7%bc%96%e8%af%91](https://uniapp.dcloud.io/platform?id=条件编译)

```js
uni.request({
// #ifdef  MP-WEIXIN
	url: 'http://localhost:8888/getbanner', //banner
// #endif
// #ifdef  H5
	url: '/api/getbanner', 
// #endif
// #ifdef APP-PLUS
	// APP需条件编译的代码
// #endif
	data: {},
	header: {},
	success: (res) => {
		// console.log(res.data);
		this.bannerList = res.data[0].my_swipe;
	}
});
```

