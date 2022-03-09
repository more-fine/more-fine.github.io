---
title: layui输入框提示不显示
date: 2020-09-29 23:00:35
tags: [bug,layui]
---

## 问题踩坑：

**在使用layui的时候，通常引入一个layui.all.js,把他放到js文件夹下，在写一些输入框的时候，表单验证的时候，提示弹窗显示不出来**

**以下是代码部分:**

```html
<div class="layui-row">
    <input type="text" name="phone" lay-verify="name" placeholder="请输入姓名" />
    <select name="sheng" lay-verify="sheng" lay-filter="sheng" required>
            <option value="">请选择省</option>
    </select>
    <select name="address" lay-verify="address" required>
            <option value="">请选择市</option>
    </select>
</div>
```

```js
//js代码layui.use(['form', 'layedit'], function () {
        var form = layui.form
            , layer = layui.layer
            , layedit = layui.layedit
        //自定义验证规则，用来验证表单输入信息的，标签要绑定lay-verify="sheng"
        form.verify({
            sheng: function (value) {
                if (value.length < 1) {
                    return '请选择省份';
                }
            },
            phone: function (phone) {
                let myreg = /^[1][3,4,5,6,7,8,9][0-9]{9}$/;
                if (!myreg.test(phone)) {
                    return "请输入正确手机号";
                }
            },
            address: function (value) {
                if (value.length < 1) {
                    return '请选择市';
                }
            },
        })
        //监听省的数据，可以获取市的信息
        form.on('select(sheng)', function (data) {
            getCityList(data.value);//查找城市
        });
})
```

**解决问题：**

 　　给一张图片你就明白了。下载的layui压缩包内找到对应的文件拷贝到你的放js的文件里面，我把css文件包放进js文件里面就解决问题了，立马弹出提示

![img](/img/2021/1809041-20200712175227595-1333758996.png)



其实代码都是一步步躺坑过来的，都有点怀疑自己了，到最后发现代码写的没有什么问题，其实问题出在用layui写页面的时候不能只仅仅引入他的js，所以他会报一些错误，js文件夹下的一些css文件找不到？？？，还有一些文件找不到，什么问题，只需要把缺少的一些文件拷贝过来就能出来你想要的结果。

博主还在继续踩坑，点滴分享，希望能够给您提供一些帮助！