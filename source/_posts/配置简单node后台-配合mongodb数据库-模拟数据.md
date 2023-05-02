---
title: '配置简单node后台,配合mongodb数据库,模拟数据'
date: 2020-09-29 23:21:08
tags: [备忘录]
---

## [配置简单node后台,配合mongodb数据库,模拟数据](https://www.cnblogs.com/MrXXD/p/13306066.html)

### 首先新建文件夹houtai

1,在当前文件夹下cmd运行,终端打开,输入命令

npm init初始化项目,几个enter确定之后,需要的一些配置
npm i express body-parser mongodb url path

然后基本的东西已经好了,

![img](/img/2021/1809041-20200715154107859-1320591019.png)

2,新建一个app.js和routes文件夹

![img](/img/2021/1809041-20200715154232379-266190731.png)

 

app.js内容如下:(备忘)

```js
const express = require("express")
const  app= express();
const bodyParser=require("body-parser")
const index = require("./routes/index")
const path = require('path');

// app.use(express.static(path.join(__dirname, 'public')));
app.use('/public',express.static('public'))
//跨域
app.all("*",function(req,res,next){
    res.header('Access-Control-Allow-Origin', '*'); 
    res.header("Access-Control-Allow-Headers", "*");
    res.header("Access-Control-Allow-Methods", "*");
    next();  
});
app.use(bodyParser.urlencoded({
    extended: false
}))
app.listen("8080",()=>{
    console.log("欢迎访问后台8080")
})

app.use("/",index)
```



routes文件夹下存放封装好的操作mongodb数据库的简单方法文件和一个存放后台接口方法的文件:

![img](/img/2021/1809041-20200715160412614-1173780135.png)

封装的调取mongodb方法文件内容如下:(备忘)其中包括要链接mongodb的库名,需要修改

**dblianjie.js**

```js
var mongo=require('mongodb'); //引入模块

var MongoClient=mongo.MongoClient; //mongodb 客户端

var url='mongodb://127.0.0.1:27017'; //mongodb服务器地址

var dbName='xxx'//要连接的库,mongodb的库名

// 查找方法
var  find=function(client, collection, data, callback){
    
    collection.find(data).toArray(function(err,result){
        if(err) throw err;
        console.log("查找成功");
        
        callback(result)//数据传回回调函数
        
        client.close();//释放连接
    })
}
// 删除一个方法{age:1}
var  del=function(client, collection, data, callback){
    collection.deleteOne(data,function(err,result){
        if (err) throw err;
        console.log("删除成功");
        callback(result);
        client.close();//释放连接
    })
}
// 增加一个方法
var insert=function(client, collection, data, callback){
    collection.insert(data,function(err,result){
        
        if(err) throw err;
        console.log("插入成功");
        
        callback(result)
        client.close();//释放连接
    })
}
// 修改一个方法
    // mongo("update","user",[{name:"老王"},{age:888}],function(){})
var update=function(client, collection, data, callback){
    collection.update(data[0],{$set:data[1]},function(err,result){
        if(err) throw err;
        console.log("修改成功");
        callback(result)
        client.close();//释放连接
    })
}
//删除多个
var deleteMany=function(client, collection, data, callback){
    collection.deleteMany(data,function(err,result){
        if (err) throw err;
        console.log("删除成功");
        callback(result);
        client.close();//释放连接
    })
}
//i 插入多个
 var insertMany=function(){
                //            {name:1}
                //         [{},{}]
     collection.insertMany(data,function(err,result){
         if(err) throw err;
         console.log("插入成功");
         callback(result)
         client.close();//释放连接
     })
 }
// 修改多个
var updateMany=function(client, collection, data, callback    ){
        collection.updateMany(data[0],{$set:data[1]},function(err,result){
            if(err) throw err;
            console.log("修改成功");
            callback(result)
            client.close();//释放连接
        })
}
var methodType={
    find:find,
    del:del,
    insert:insert,
    update:update,
    deleteMany:deleteMany,
    insertMany:insertMany,
    updateMany:updateMany
}

                    // find    表           条件    回调函数  
module.exports=function(type, collections, data, callback){
    // 连接数据库
    MongoClient.connect(url, function(err, client) {
        if (err) {
            console.log('链接失败');
            console.log(err);
            
        } else { // 链接成功
            var db = client.db(dbName); // 对数据库的链接
            
            var collection = db.collection(collections); // 对集合的链接
                    
            methodType[type](client, collection, data, callback);
            
            
        }
        
    })
    
    
}


```

index.js基本内容如下:(备忘)

```js
const express = require("express")
 const router = express.Router()
 const mongodb = require("mongodb")
 //需要可以自己下依赖
 // const fs = require("fs")
 // const jet = require("../jsonwebtoken")
 const mongo = require("./dblianjie")
 const ObjectId = mongodb.ObjectId
 
 
 // // 用户信息列表
 // router.get("/findUser", (req, res) => {
 //     mongo("find", "userList", { iphone: req.query.name }, (result) => {
 //         console.log(result)
 //         res.send(result)
 //     })
 // })
 
 //获取首页数据
 // router.get("/getIndex", (req, res) => {
//     mongo("find", "tuiJ", {}, (result) => {
//         res.send(result)
 //     })
 // })
 //导出
 module.exports = router 

```

### 后台使用命令node app就可以启动啦

前台拿到数据就可以对数据进行基本的操作了



```js
//请求后台接口就行啦
    axios({//getIndex就是后台配置的接口地址 url: "http://localhost:8080/getIndex",
      params: "",
      method: "get"
    }).then(res => {
      console.log(res)
    });
```