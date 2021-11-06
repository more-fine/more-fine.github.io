---
title: 下拉刷新加载数据，筛选条件点击加载对应数据，搜索input框得到对应数据
date: 2020-10-06 12:58:34
tags: [小栗子,layui]

---

### [下拉刷新加载数据，筛选条件点击加载对应数据，搜索input框得到对应数据](https://www.cnblogs.com/MrXXD/p/13429544.html)

#### 移动端页面的下拉刷新，用到jquery和layui的弹窗，layui的下拉刷新配合上条件筛选有点难搞，所以没有用，原生js写的，有用到的话可以采纳

**html部分：**

```html
<div class="seo_Box">
                <input class="case_search" type="text" placeholder="大家都在搜“现代风格”" />
</div>

//筛选条件，点击出现弹窗
<ul class="search_list">
                <li class="searchLi" data-method="offsetFG" data-type="b">
                    风格
                    <img src="images/bottom.png" alt="">
                </li>
                <li class="searchLi" data-method="offsetHX" data-type="b">
                    户型
                    <img src="images/bottom.png" alt="">
                </li>
                <li class="searchLi" data-method="offsetMJ" data-type="b">
                    面积
                    <img src="images/bottom.png" alt="">
                </li>
                <li class="searchLi" data-method="offsetKJ" data-type="b">
                    空间
                    <img src="images/bottom.png" alt="">
                </li>
                <li class="searchLi" data-method="offsetZJ" data-type="b">
                    造价
                    <img src="images/bottom.png" alt="">

                </li>
            </ul>

<!-- 内容列表 -->
        <div class="case_main">
            <ul class="newList" id="LAY_List">

            </ul>
        </div>
```

**js部分：**

```js
 // 加载搜索条件数据
    $.ajax({
        methods: "get",
        url: ajaxUrl + "mobile/website/caseInfo/sysDictItem",
        success: function (res) {
            let data = res.result
            showSearchData(data)//初始化渲染搜索条件
        }
    })
//layui弹窗
layui.use('layer', function () {
        var layer = layui.layer
        //触发事件
        var active = {

            offsetFG: function (othis) {
                var type = othis.data('type')
                    , text = othis.text();

                layer.open({
                    type: 1
                    , area: ['3.55rem', '2.5rem']
                    // , area: ['100%', '300px']
                    , title: "请选择房屋风格"
                    // , offset: 'auto'//具体配置参考：http://www.layui.com/doc/modules/layer.html#offset
                    , id: 'layerDemo' + type //防止重复弹出
                    , content: text1
                    , shadeClose: true//点击遮罩关闭
                    , btn: '确定'
                    , btnAlign: 'c' //按钮居中
                    , shade: .5 //不显示遮罩
                    , yes: function () {
                        layer.closeAll();
                    }
                });
            },
            offsetHX: function (othis) {
                //同上面弹窗一样
                  //content: text2
            },
            offsetMJ: function (othis) {
                //content: text3
                
            },
            offsetKJ: function (othis) {
                   //content: text4
                 
            },
            offsetZJ: function (othis) {
               //content: text5
            },

        }


$('.searchLi').on('click', function () {
            var othis = $(this), method = othis.data('method');
            active[method] ? active[method].call(this, othis) : '';
            let index = $(this).index();//当前点击的index
            // 点击搜索条件加载数据
            $('.search_data li').click(function () {
                // console.log($(this).index())
                // console.log(index)
                $(this).addClass('redBg').siblings("li").removeClass('redBg')//更改样式Class
                // console.log($(".search_list").children(".searchLi").eq(index).html())
                // $(".search_list").children(".searchLi").eq(index).html($(this).html()+`<img src="images/bottom.png" style="margin-left:.05rem;" alt="">`)
                $(".search_list").children(".searchLi").eq(index).css("color", "red")
                // console.log($(this).parent().html())
                // 更改text1等内容，不然会一直重复渲染初始化的dom
                if (index == 0) {
                    text1 = '<ul class="search_data">' + $(this).parent().html() + '</ul>'
                } else if (index == 1) {
                    text2 = '<ul class="search_data">' + $(this).parent().html() + '</ul>'
                } else if (index == 2) {
                    text3 = '<ul class="search_data">' + $(this).parent().html() + '</ul>'
                } else if (index == 3) {
                    text4 = '<ul class="search_data">' + $(this).parent().html() + '</ul>'
                } else if (index == 4) {
                    text5 = '<ul class="search_data">' + $(this).parent().html() + '</ul>'
                }
            })

            searchClick(index);//更改渲染条件

        });
    })




//////////////////////////////////////////////////////////////////////
//公用的请求参数
    var sxData = { upAndDown: 0, caseState: 1, pageNo: 0, status: 1, pageSize: 12 }
    var isloading = false;//是否滑动到底部
    var dataPages = 0;//总页数
    var searchButton = false;//是否点击赛选条件

    var p = 0, t = 0; //上下滚动时隐藏导航栏
//鼠标滚轮事件
    $(window).scroll(function (e) {
        //下滑
        p = $(this).scrollTop();
        if (t < p && p !== 0) {
            $('.header_right ul').slideUp('fast');
        }
        // else {
        //     //上滑
        //     $('.header_right ul').slideDown('fast');
        // }
        t = p;

        var scrtop = document.documentElement.scrollTop || document.body.scrollTop;
        var wheight = document.documentElement.clientHeight || document.body.clientHeight;
        var scrheight = document.documentElement.scrollHeight || document.body.scrollHeight;
        var windowheight = window.innerHeight


        // console.log(isloading, scrtop, wheight, scrheight)

        if (scrtop + wheight >= (scrheight - 10) && isloading == false) {
            if (sxData.pageNo >= dataPages) {
                // console.log('没有更多了')
                return false
            }
            isloading = true //正在请求值改为真，请求完成之后改为假
            getData()
        }

    })


    getData();//初始化加载数据
    function getData() {
        sxData.pageNo++
        $.ajax({
            type: "get",
            dataType: "json",
            data: sxData,//请求的页码和每页显示条数
            async: true,
            url: Url + 'xxx/xxx/xxx/list',//Url地址
            success: function (res) {
                if (res.success && res.result.records.length >= 0) {//数据插入
                    isloading = false
                    dataPages = res.result.pages;
                    if (searchButton) {//点击了赛选按钮
                        var html = '';
                    } else {
                        var html = $("#LAY_List").html()
                        searchButton = false;
                    }
                    res.result.records.forEach( function (item) {
                        html += `
                                <li pid="${item.id}">
                                    <h1 style="background: url('${(item.headFiles.length == 0) ? ('') : (item.showRootPath + item.headFiles[0].fileUrl)}') no-repeat; background-size: 100% 100%;"></h1>
                                    <h2>${item.title}</h2>
                                </li>
                                `
                    });

                    $("#LAY_List").html(html)
                    $("#LAY_List li").click(function () {
                        location.href = "caseDetails.html?id=" + $(this).attr("pid");//跳转页面携带id
                    })
                }
            },
            error: function () {
                //请求出错处理
                isloading = false
            }
        })
    }




    /////////////////////////////////////////////////////////////////////////
    
    var text1, text2, text3, text4, text5 = ''
    function showSearchData(data) {
        text1 = `<ul class="search_data"><li class="redBg" pid="null">全部</li>`
        text2 = `<ul class="search_data"><li class="redBg" pid="null">全部</li>`
        text3 = `<ul class="search_data"><li class="redBg" pid="null">全部</li>`
        text4 = `<ul class="search_data"><li class="redBg" pid="null">全部</li>`
        text5 = `<ul class="search_data"><li class="redBg" pid="null">全部</li>`

        for (let i = 0; i < data.designStyle.length; i++) {
            text1 += `<li pid="${data.designStyle[i].id}">${data.designStyle[i].itemText}</li>`
        }
        text1 += `</ul>`
        for (let i = 0; i < data.apartmentLayout.length; i++) {
            text2 += `<li pid="${data.apartmentLayout[i].id}">${data.apartmentLayout[i].itemText}</li>`
        }
        text2 += `</ul>`

        for (let i = 0; i < data.houseArea.length; i++) {
            text3 += `<li pid="${data.houseArea[i].id}">${data.houseArea[i].itemText}</li>`
        }
        text3 += `</ul>`

        for (let i = 0; i < data.housingFeatures.length; i++) {
            text4 += `<li pid="${data.housingFeatures[i].id}">${data.housingFeatures[i].itemText}</li>`
        }
        text4 += `</ul>`

        for (let i = 0; i < data.cost.length; i++) {
            text5 += `<li pid="${data.cost[i].id}">${data.cost[i].itemText}</li>`
        }
        text5 += `</ul>`

    }





    function searchClick(index) {
        if (index == '0') {
            $(".search_data li").click(function () {//风格
                let designStyle = $(this).attr("pid")
                if (designStyle != "null") {
                    sxData.designStyle = designStyle;
                } else {
                    delete sxData.designStyle
                }
                sxData.pageNo = 0;//重新定义到搜索
                searchButton = true;//点击了赛选条件
                getData()
            })
        } else if (index == '1') {
            $(".search_data li").click(function () {//户型
                let houseType = $(this).attr("pid")
                if (houseType != "null") {
                    sxData.houseType = houseType;
                } else {
                    delete sxData.houseType
                }
                searchButton = true;//点击了赛选条件
                sxData.pageNo = 0;//重新定义到搜索
                getData()
            })
        } else if (index == '2') {
            $(".search_data li").click(function () {//面积
                let floorSpace = $(this).attr("pid")
                if (floorSpace != "null") {
                    sxData.floorSpaceType = floorSpace
                } else {
                    delete sxData.floorSpaceType
                }

                searchButton = true;//点击了赛选条件
                sxData.pageNo = 0;//重新定义到搜索
                getData()
            })
        } else if (index == '3') {
            $(".search_data li").click(function () {//空间
                let feature = $(this).attr("pid")
                if (feature != "null") {
                    sxData.feature = feature;
                } else {
                    delete sxData.feature
                }
                searchButton = true;//点击了赛选条件
                sxData.pageNo = 0;//重新定义到搜索
                getData()
            })
        } else if (index == '4') {
            $(".search_data li").click(function () {//费用
                let cost = $(this).attr("pid")
                if (cost != "null") {
                    sxData.costType = cost;
                } else {
                    delete sxData.costType
                }
                searchButton = true;//点击了赛选条件
                sxData.pageNo = 0;//重新定义到搜索
                getData()
            })
        }


    }
    // // 监听键盘
    document.addEventListener("keyup", function (e) {
        // console.log(e.keyCode)
        if (e.keyCode == 13) {
            // console.log("按下了enter")
            let title = $(".case_search").val()
            // console.log(title)
            sxData.title = title
            searchButton = true;//点击了赛选条件
            sxData.pageNo = 0;//重新定义到搜索
            getData()
        }
    })
```

