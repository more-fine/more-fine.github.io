---
title: vue中路由跳转传递参数的三种方式
date: 2020-09-18 00:15:04
tags: vue
categories: 
---

# vue中路由跳转传递参数的三种方式


日常业务中，路由跳转的同时传递参数是比较常见的，传参的方式有三种：

## 1）通过动态路由方式

//路由配置文件中 配置动态路由
{
     path: '/detail/:id',
     name: 'Detail',
     component: Detail
   }
//跳转时页面
var id = 1;
this.$router.push('/detail/' + id)

//跳转后页面获取参数
this.$route.params.id

## 2）通过query属性传值

//路由配置文件中
{
     path: '/detail',
     name: 'Detail',
     component: Detail
   }
//跳转时页面
this.$router.push({
  path: '/detail',
  query: {
    name: '张三',
    id: 1,
  }
})

//跳转后页面获取参数对象
this.$route.query 

## 3）通过params属性传值

//路由配置文件中
{
     path: '/detail',
     name: 'Detail',
     component: Detail
   }
//跳转时页面
this.$router.push({
  name: 'Detail',
  params: {
    name: '张三'，
    id: 1,
  }
})

//跳转后页面获取参数对象
this.$route.params 

## 总结：

1.动态路由和query属性传值 页面刷新参数不会丢失， params会丢失 2.动态路由一般用来传一个参数时居多(如详情页的id), query、params可以传递一个也可以传递多个参数 。