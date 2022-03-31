---
title: 面试总结
tags: vue
date: 2020-10-1 20:15:04
categories: 知识点总结
---
# 面试总结

# vue部分:

## 0.vue开发的时候遇到的坑

1、vue动态设置img的src属性不生效

​				使用require引入图片路径

2、异步组件实现按需加载

app.js很大的时候采用这种方法。

我们每次访问一个页面的时候，如果不采取按需加载资源文件（css和js），就会造成页面访问速度变慢，因为它会一次性加载所有项目资源。这时候，就需要用异步组件的方式，实现按需加载了。

3.有时候希望在页面渲染完成之后，再来执行某个函数方法比如获取某个列表的高度时，必须要在页面完全渲染之后才可以，页面没有加载完成之前，获取到的高度不准确。

Vue文档中有个mounted函数，说是挂载到实例上去之后调用该钩子，但是经检测，函数放在mounted里执行时，高度并不正确，官方文档上还有一个this.$nextTick(),发现用这个的话还是不太准确,最后:
发现可以通过watch和nextTick 来达到我想要的效果
headImgList 是我要监听的列表数组，当他全部加载结束之后再调用 pcGlasses() 方法
如下:

```
watch(){
    headImgList:function(){
        this.$nextTick(function(){
            this.pcFlasses()
        })
    }
}
```



## 1.组件通信

#### 父传子:

通过v-bind:"新属性",绑定一个新属性,以属性的方法传递到子组件中,子组件通过props进行接收

```
父组件:
<hello :aaa="info"></hello>
子组件:
props:["aaa"](跟data同级),输出==>{{aaa}}
```

#### 子传父:

子组件通过$emit()方法进行传递数据.父组件通过$emit方法的第一个参数绑定事件,然后进行回调,接收数据.

$emit():发送数据,接收两个参数,第一个参数是监听函数名,需要绑定监听方法,第二个参数是传递的数据

```html
//子组件中:
mounted(){
	this.$emit("fn",this.msg)
}
//父组件中:
<hello @fn="add"></hello>

methods:{
	add(val){
		this.value = val;
		console.log(val)
	}
}
```

#### 同级组件之间:

1.可以通过子传父,在进行父传子

2.bus事件总线,思想源于java，相当于公交车，Bus.$emit上车，Bus.$on下车接收数据

3.vuex进行传值

4.本地存储

```html
//a组件传给b组件:(同级)
//a组件中:
methods: {
     sendMessage () {//上车
    	Bus.$emit('inceptMessage', this.msg) 
	}
}

//b组件中:
 mounted () {//下车
     Bus.$on('inceptMessage',(msg) => {
        this.fromComponentAMsg = msg
     })
}

```



#### 页面之间组件通信:

1.本地存储

**2.路由传值**

3.vuex

4.$emit()和$on配合

总结:

新组件等待旧组件销毁后,旧组件才发送数据.

旧组件等待新组件挂载前,新组建才接收数据.



可以接收数据的声明周期:beforMOunted,created,beforCreated

可以发送数据的生命周期:beforDestrog(),destroyed()

## 2.vue的数据双向绑定原理:

1.

通过es5中的object.defineProperty()进行数据劫持,这个方法内部封装了两个属性,一个是描述符,和存取描述符,set,setter,get,getter,配合observer观察者模式进行对数据的监听,然后通过watch进行通知compile解析指令,然后更新页面.

2.

```
通过Object.defineProperty（）的 set 和 get，监听对数据的操作，从而触发数据同步。

Object.defineProperty()缺陷：
1、只能对属性进行数据劫持，并且需要深度遍历整个对象
2、对于数组不能监听数据的变化
而proxy(es6的方法)支持监听数组的变化，并且可以直接对整个对象进行拦截。
```

3.

```
执行以下3个步骤，实现数据的双向绑定：
   1.实现一个监听器Observer，用来劫持并监听所有属性，如果有变动的，就通知订阅者。
   2.实现一个订阅者Watcher，可以收到属性的变化通知并执行相应的函数，从而更新视图。
   3.实现一个解析器Compile，可以扫描和解析每个节点的相关指令，并根据初始化模板数据以及初始化相应的订阅器

```



## 3.路由传值的区别:

```
 path传值特点:

​     1.不可以传对象

​     2.动态路由  path:公共组件儿/:传值

​     3.传值会暴露在地址栏中   get

query传值的特点  

​     1.配合path动态路由  一个公共组件儿搭载多个路由文件

​     2.query传值可以传对象

​     3.在地址栏中暴露

params传值 特点

​     1.配合我们的命名路由搭配使用

​     2.不能配合path动态路由

​     3.刷新页面丢失

​     4.命名路由按照名字查找对应的router-link文件

​     5.传值不展示在地址栏中
```

**写法:**

```
path:
{
 path: '/child/:id',
 component: Child
}

<router-link :to="/child/1"> 跳转到子路由 </router-link>

this.$router.push({
  path:'/child/${id}',
})

query:
{
 path: '/child,
 name: 'Child',
 component: Child
}

<router-link :to="{name:'Child',query:{id:1}}">跳转到子路由</router-link>

this.$router.push({
  name:'Child',
  query:{
   id:1
  }
})

params:
{
 path: '/child,
 name: 'Child',
 component: Child
}

<router-link :to="{name:'Child',params:{id:1}}">跳转到子路由</router-link>

this.$router.push({
  name:'Child',
  params:{
   id:1
  }
})
```



## 4.什么是虚拟DOM

虚拟DOM不会进行排版与重绘操作,虚拟DOM相当于在js和真实dom中间加了一个缓存,利用dom diff算法避免了没有必要的dom操作,从而提高性能.

具体实现步骤:
		1.用JavaScript对象结构表示DOM树的结构然后用这个树构建一个真正的DOM树,插到文档中
		2.当状态变更的时候,重新构造一棵树的对象树,然后用新的树和旧的树行对比,记录两棵树差异
		3.把2所记录的差异应用到步骤1所构建的真正的DOM树上,视图就更新了

## 5.单页面应用(SPA)

**单页面应用指的是只有一个主页面的应用,包含了所有的(html,css,js).所有的页面内容都包含在了这个所谓的主页面中,在书写的时候,页面片段还是分开写的,然后在交互的时候通过路由程序动态的载入,单页面的应用在进行页面跳转的时候,只刷新他的局部资源,多用于pc端的开发.**

单页面应用的特点:

1,用户体验比较好,快,内容的改变不需要重新加载整个页面,对服务器的压力比较比较小.

2,前后端分离

3,可以设置一些比较炫酷的动画(比如切换页面的是后设置一些专场动画什么的)

单页面应用的缺点:

1,不利于seo

2,导航不可用,如果一定要设置导航的话,需要自行的实现前进,后退的功能（由于是单页面不能用浏览器的前进后退功能，所以需要自己建立堆栈管理）

3,初次加载的时候比较耗的时间比较多

4,页面的复杂度提高了很多

多页面（MPA），就是指一个应用中有多个页面，页面跳转时是整页刷新。

多页面的缺点：
页面切换加载缓慢，用户体验不好，流畅度差。

## 6.for...in和for...of的区别

for...of是es6中的新方法,修改了es5中for...in的不足

for...in循环出的是key,for...of循环出的是value

for...of不能循环普通的对象,需要通过object,keys()搭配使用

推荐在循环对象的时候使用for...in,在循环数组的时候使用for...of



## 7.vuex的数据流向

只用来读取的状态,集中放在store中； 改变状态的方式是提交mutations，这是个同步的事物； 异步逻辑应该封装在action中。

**页面的组件通过Dispatch 来调用Vuex的Actions 进行Actions逻辑处理数据，将处理好的数据通过commit 进行Mutations 中的数据逻辑处理,并且修改state中的存储的数据，state 是响应式的，只要state中的数据发生修改，会重新渲染组件.**

**vuex中的五种属性:**

分别是 State、 Getter、Mutation 、Action、 Module

**state**
Vuex 使用单一状态树,即每个应用将仅仅包含一个store 实例，但单一状态树和模块化并不冲突。存放的数据状态，不可以直接修改里面的数据。

**getters**
类似vue的计算属性，主要用来过滤一些数据。

**action**
actions可以理解为通过将mutations里面处里数据的方法变成可异步的处理数据的方法，简单的说就是异步操作数据。view 层通过 store.dispath 来分发 action。

**mutations**

定义的方法动态修改Vuex 的 store 中的状态或数据。

**Module**

相当于vuex的一个类;各个模块之间的vuex的操作都可以进行划分,各模块的方法好区分及维护;





**vuex 中核心原理**：通过vue 实例，将state数据赋值给data()

**vuex的State特性是？**
1、**Vuex就是一个仓库**，仓库里面放了很多对象。涉及多个组件储存的公共数据
其中state就是数据源存放地，对应于与一般Vue对象里面的data
2、**state里面存放的数据是响应式的**，Vue组件从store中读取数据，若是store中的数据发生改变，依赖这个数据的组件也会发生更新
3、它通过mapState把全局的 state 和 getters 映射到当前组件的 computed 计算属性中

**vuex的Getter特性是？**
1、getters 可以对State进行计算操作，它就是Store的计算属性
2、 虽然在组件内也可以做计算属性，但是**getters 可以在多组件之间复用**
3、 如果一个状态只在一个组件内使用，是可以不用getters

**Action 的Mutation区别是？**
1、Action 类似于 mutation，不同在于：
2、Action 提交的是 mutation，而不是直接变更状态。
3、Action 可以包含任意异步操作。

4、定义的方法动态修改Vuex 的 store 中的状态或数据,同步操作。



## 8.computed和watch区别

**computer** 计算属性.通过计算得出的属性就是计算属性

计算属性可以是一个函数或者是一个getter和setter组成的对象

**watch** 监听/侦察.当数据变化时执行一个函数

watch属性可以是字符串、函数、对象、数组

拥有**deep，immediate**两属性

**computed（计算属性）有缓存**
watch时刻监听data中的数据，无缓存，一旦在跟组件中写**watch，则一直时刻监听，浪费性能**

一般的话不推荐使用watch()

**computed 和 watch 的使用场景不一样**
	computed 是通过几个数据的变化，来影响一个数据，
	watch是可以一个数据的变化，去影响多个数据。  

**computed和methods的区别:**
    computed是属性调用，通过`{{computed方法名}}`就可以调用
    methods是函数调用，需要通过`{{methods方法名+()}}`才可以调用



## 9、什么是闭包 

​	闭包是在函数里面定义一个子函数，子函数可以是匿名函数，该子函数能够读写父函数的局部变量,

​	在使用闭包的过程中**不会释放外部的引用**，**闭包函数内部的值会得到保留**。
​	闭包里面的匿名函数，读取变量的顺序，**先读取本地变量，再读取父函数的局部变量**。
​	对于**闭包外部无法引用它内部的变量**，所以说在函数内部创建的变量**执行完后要立刻释放资源**(也就是说把里面的变量清掉)，这样的话:不污染全局对象
​	如果**使用不当**的情况下可能会**导致内存泄漏**，因为没有释放外部引用，但是合理的使用闭包是内存使用不是内存泄漏







## 10.new一个对象,这个过程发生了什么?

var obj = new Object("name","zhangsan")

创建一个新对象,如:var obj = {};

然后将新对象的proto属性指向构造函数的原型对象.

将构造函数的作用域赋值给新对象,**就是改变this指向**(将this指向新的对象)

执行构造函数内部的代码,新属性添加给obj的this对象.

最后返回新的对象.



## 11.原型和原型链与原型对象和constructor属性

​	prototype：此属性只有构造函数才有，这个属性指向实例对象的**原型对象**,通过构造函数生成实例的时候都会为实例分配prototype(原型对象)。
​	__proto__：这个是任何对象在创建时都会有的一个属性，它指向了产生**当前对象的构造函数的原型对象**，由于并非标准规定属性，不要随便去更改这个属性的值，以免破坏原型链，但是可以借助这个属性来学习，所谓的原型链就是由__proto__连接而成的链。

当读取某个对象的某个属性时都会执行一次搜索,首先从实例中找,如果没有找到的话,就去原型的原型上去找,就这样一直找到最顶层的Object.prototype,还是找不到,就会返回一个undefined

**constructor**:属性执行创建实例对象的构造函数

## 12.箭头函数this指向与普通函数的区别

箭头函数中的this指向:定义函数所在位置的对象
    不可以当作构造函数 不能使用new关键字
    不可以使用arguments对象（伪数组  ...扩展运算符：作用copy数组）
区别：

​	**普通函数中this指向可以改变，普通函数中this谁调用指向谁**

​	**箭头函数中的this是固定的,指向定义函数所在位置**

**改变this指向的方法:**

https://www.jianshu.com/p/b86002cc4639

​	1,call()方法,调用方法fun.call(thisArg, arg1, arg2, ...)

​	2,apply()方法,调用方法fun.apply(thisArg, [argsArray])

​	3,bind()方法,fun.bind(thisArg, arg1, arg2, ...)

区别:

apply() 与call（）非常相似，不同之处在于提供参数的方式，apply（）使用参数数组，而不是参数列表

bind()在react中常用,不会调用函数, 可以改变函数内部this指向.


## 13.android 和   ios开发中遇到的问题 

**(1)、iOS 滑动不流畅**
	q:上下滑动页面会产生卡顿，手指离开页面，页面立即停止运动。整体表现就是滑动不流畅，没有滑动惯性。

1.在滚动容器上增加滚动 touch 方法

将-webkit-overflow-scrolling 值设置为 touch

2.设置 overflow

设置外部 overflow 为 hidden,设置内容元素 overflow 为 auto。内部元素超出 body 即产生滚动，超出的部分 body 隐藏。

**(2)、iOS 上拉边界下拉出现白色空白**
	q：手指按住屏幕下拉，屏幕顶部会多出一块白色区域。手指按住屏幕上拉，底部多出一块白色区域。
	在 iOS 中，手指按住屏幕上下拖动，会触发 touchmove 事件。这个事件触发的对象是整个 webview 容器，容器自然会被拖动，剩下的部分会成空白。

a:监听事件禁止滑动
	滚动妥协填充空白，装饰成其他功能

**(3)、click 点击事件延时与穿透**

使用 fastclick 库

**(4)、软键盘将页面顶起来、收起未回落问题**

q:Android 手机中，点击 input 框时，键盘弹出，将页面顶起来，导致页面样式错乱。移开焦点时，键盘收起，键盘区域空白，未回落。

a:软键盘将页面顶起来的解决方案，主要是通过监听页面高度变化，强制恢复成弹出前的高度。

## 14.require 和  import 区别 

**遵循规范**
–require 是 AMD规范引入方式
–import是es6的一个语法标准，如果要兼容浏览器的话必须转化成es5的语法

**调用时间**

–require是运行时调用，所以require理论上可以运用在代码的任何地方

–import是编译, 时调用，所以必须放在文件开头

**本质**

–require是赋值过程，其实require的结果就是对象、数字、字符串、函数等，再把require的结果赋值给某个变量

–import是解构过程，但是目前所有的引擎都还没有实现import，我们在node中使用babel支持ES6，也仅仅是将ES6转码为ES5再执行，import语法会被转码为require

**node的module遵循CommonJS规范，requirejs遵循AMD，seajs遵循CMD，虽各有不同，但总之还是希望保持较为统一的代码风格。**

## 15.Hash和history的区别

hash模式的原理是onhashchange事件

1,地址栏url中显示

​	hash有#显示,history没有;**vue默认使用hash**

2,请求方式

​	hash的#虽然出现在url中,但是不会包含在http请求中,对后端没有影响,因此改变hash不会重新加载页面

​	在history模式中，刷新一下网页，明显可以看到请求url为完整的url，url完整地请求了后端

3,history模式下:**提供了对历史记录进行修改的功能, back、forward、go方法**

​	利用了 HTML5 History Interface 中新增的 pushState() 和 replaceState() 方法。（需要特定浏览器支持）
这两个方法应用于浏览器的历史记录栈，在当前已有的 back、forward、go 的基础之上，它们提供了对历史记录进行修改的功能。只是当它们执行修改时，虽然改变了当前的 URL，但浏览器不会立即向后端发送请求。

通过history api,可以对浏览器的历史记录栈进行修改,但是只要刷新页面就会加载请求,（如果后端没有准备的话），则会刷新出来404页面。

## 16.MVVM和MVC

anjular--mvvm

vue--mvvm

react--mvc
​	MVC 和 MVVM 都是一种设计思想 	mvc中 的  controller 演变成 MVVM中的 viewmodel  
​	mvvm解决了 MVC中大量的DOM操作使得页面渲染性能变低，加载速度变低，影响用户体验。

[MVC](https://so.csdn.net/so/search?q=MVC&spm=1001.2101.3001.7020)**设计模式**

- M - Model  数据：数据实体,用来保存页面要展示的数据.
- V - View      视图：负责显示数据的,一般其实就是指的html页面.
- C - Controller 控制器： 控制整个业务逻辑,负责处理数据,比如数据的获取,以及数据的过滤，进而影响数据在视图上的展示.

[MVVM](https://so.csdn.net/so/search?q=MVVM&spm=1001.2101.3001.7020)**设计模式**

- M - Model 数据：它是与应用程序的业务逻辑相关的数据的封装载体
- V - View 视图：它专注于界面的显示和渲染
- VM - ViewModel 视图-数据：它是View和Model的粘合体，负责View和Model的交互和协作

[Angular](https://so.csdn.net/so/search?q=Angular&spm=1001.2101.3001.7020)是MVC还是**MVVM**

- MVC的界面和逻辑关联紧密，数据直接从数据库读取。MVVM的界面与viewmodel是松耦合，界面数据从viewmodel中获取。所以angularjs更倾向于mvvm

**MVVM的优点**

1. 低耦合：View可以独立于Model变化和修改，同一个ViewModel可以被多个View复用；并且可以做到View和Model的变化互不影响；
2. 可重用性：可以把一些视图的逻辑放在ViewModel，让多个View复用；
3. 独立开发：开发人员可以专注与业务逻辑和数据的开发
   ViewModemvvm设计人员可以专注于UI(View)的设计；
4. 可测试性：清晰的View分层，使得针对表现层业务逻辑的测试更容易，更简单。

## 17.移动端的性能优化 

​	1、首屏加载 按需加载  懒加载
​	2、资源预加载

## 18、从输入 URL 到获取页面的完整过程

​	1、域名解析(查询DNS),获取域名对应的 IP地址，查询浏览器缓存
​	2、浏览器与服务器建立TCP连接
​	3、浏览器向服务器发送http 请求
​	4、服务器接收到请求后，根据路径参数，经过后端的一些处理,生成html代码 返回浏览器
​	5、浏览器拿到完整的HTML页面代码开始解析和渲染 
​	6、浏览器根据拿到的资源对页面进行渲染 把一个完整的页面渲染出来

## 三次握手,四次挥手

三次握手是建立连接,四次挥手是关闭一个TCP连接

**三次握手:**

1,客户端==>服务端(连接请求)

2,服务端==>客户端(授予请求)

3,客户端==>服务端(确认请求)

**四次挥手:**

客户端：我数据传完了，我要下线了
服务器：知道了，我还有点数据，你等下
   一段时间后:
服务器：我数据发完了，你可以下线了
客户端：好的
	首先是客户端发送给服务端的，发送请求报文我要断开了，此时，服务器接受到消息，就知道客户端想要和我断开，会立刻返回一个确认报文给客户端，因为此时服务器可能还有一些信息没有处理完成，就说，你等等，然后稍等片刻会给客户端发送一个消息，就说我这边消息已经发完了，你可以断开了，但是此时服务器不知道客户端是否能够收到消息，所以说最后客户端会返回一个确认报文,这样服务端才知道客户端是收到了同意分手的消息的，只有这样才能保证接的安全性，少一次都不行。

https://blog.csdn.net/whuslei/article/details/6667471

## 19、前端优化方法　　

​	1、减少HTTP请求：通过将多个前端资源合并成一个实现减少HTTP请求提高性能。　　
​	2、设置响应头字段是部分及时性要求不高的静态资源在缓存在前端浏览器中。　　 
​	3、启用传输压缩。例如gzip。　　 
​	4、合理的布局前端代码结构，css，html，js代码的顺序由上至下。　　 
​	5、对于一些可公开访问的资源，可以通过设置其他的域名的方式减少传输过程中的cookie。　　 
​	6、使用CDN分发，将静态资源部署在各大网络运营商的机房中，这样子用户就可以非常快的就近获得资源。　　
​	7、使用反向代理将热门内容，静态资源或者一些可被缓存的计算结果缓存在代理服务器中。通过配置代理服务器可以实现代理服务器直接转发被缓存的资源。

## 20、keep-alive

keep-alive:主要用于保留组件状态或避免重新渲染。
组件的缓存也是基于VNode节点的而不是直接存储DOM结构。它将满足条件（pruneCache与pruneCache）的组件在cache对象中缓存起来，在需要重新渲染的时候再将vnode节点从cache对象中取出并渲染。
被keep-alive包含的组件/路由，会多出两个生命周期：actived和deactived，actived在组件第一次渲染时会被调用，之后调用每次缓存组件被激活时调用机制，第一次进入缓存路由/组件，在mounted后面，beforeRouteEnter守卫传给next的回调函数之前调用

include包含的组件会被进行缓存，exclude包含的组件不会被缓存

## 21、diff算法

1.把树形结构按照层级分解,只比较同级元素
​	2.给列表结构的每个单元添加key属性,方便比较。在实际代码中,会对新旧两棵树进行一个深度优先的遍历,这样每个节点都会有一个标记
​	3.在深度优先遍历的时候,每遍历到一个节点就把该节点和新的树进行对比。如果有差异的话就记录到一个对象里面
​	Vritual DOM算法主要实现上面步骤的三个函数: element，diff，patch。然后就可以实际的进行使用
​	react只会匹配相同的class的component(这里的class指的是组件的名字)
​	合并操作,调用component的setState方法的时候，React将其标记为dirty。到每一个时间循环借宿，React检查所有标记dirty的component重新绘制
​	4.选择性子树渲染,可以重写shouldComponentUpdate提高diff的性能

## 22、浏览器兼容性问题

1.不同浏览器的默认margin和padding不同
解决方案：css里加通配符*{margin：0；padding：0}
2.图片默认有间距
解决方案：使用float（浮动）给图片布局
3.边距重叠问题；就是当相邻两个元素都设置了margin边距时，margin将取最大值，舍弃最小值
解决方案：
外层元素padding代替
	外层元素 overflow:hidden;
	内层元素绝对定位 postion:absolute:
	内层元素 加float:left;或display:inline-block;
	内层元素padding:1px;
	内层元素透明边框 border:1px solid transparent;
4.const声问题  就是Firefox（火狐浏览器）下，可以使用const关键字来定义常量；IE下，只能使用var关键字来定义常量。
		解决方法：统一使用var关键字来定义常量。



## 23、vue-router钩子函数

​	全局的路由钩子函数：beforeEach、afterEach

```
单个的路由钩子函数：beforeEnter

组件内的路由钩子函数：beforeRouteEnter、beforeRouteLeave、beforeRouteUpdate
```

**beforeEnter** 有三个参数：

> to: 路由将要跳转的路径信息，信息是包含在对像里边的。
>
> from: 路径跳转前的路径信息，也是一个对象的形式。
>
> next: 路由的控制参数，常用的有next(true)和next(false)。

**beforeRouteEnter**

> 在路由进入前的钩子函数
>
> 不！能！获取组件实例 `this`
>
> 因为组件实例还没被创建

**beforeRouteUpdate**

> 当前路由改变，该组件被复用时
>
> 对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
>
> 可以访问组件实例 `this`

**beforeRouteLeave**

> 在路由离开前的钩子函数
>
> 可以访问组件实例 `this`

## 24、VUE与React的区别

相同点：
1.React采用特殊的JSX语法，Vue.js在组件开发中也推崇编写.vue特殊文件格式，对文件内容都有一些约定，两者都需要编译后使用；

2.中心思想相同：一切都是组件，组件实例之间可以嵌套；都提供合理的钩子函数，可以让开发者定制化地去处理需求；都不内置列数AJAX，Route等功能到核心包，而是以插件的方式加载；在组件开发中都支持mixins的特性。
不同点：
React采用的Virtual DOM(虚拟dom,dom树的虚拟表现)会对渲染出来的结果做脏检查；Vue.js在模板中提供了指令，过滤器等，可以非常方便，快捷地操作Virtual DOM。

1.数据更改的方面:
所以在react中，是单向数据流，react在setState之后会重新走渲染的流程，如果shouldComponentUpdate返回的是true，就继续渲染，如果返回了false，就不会重新渲染.

而vue的思想是响应式的，也就是基于是数据可变的，通过对每一个属性建立Watcher来监听，当属性变化的时候，响应式的更新对应的虚拟dom。

react的性能优化需要手动去做，而vue的性能优化是自动的，但是vue的响应式机制也有问题，就是当state特别多的时候，Watcher也会很多，会导致卡顿，所以大型应用（状态特别多的）一般用react，更加可控
2.通过js来操作一切，还是用各自的处理方式
react的思路all in js,是通过js来生成html，所以设计了jsx，还有通过js来操作css，社区的styled-component、jss等，

vue是把html，css，js组合到一起，用各自的处理方式，vue有单文件组件，可以把html、css、js写到一个文件中，html提供了模板引擎来处理。



## 25、route和router的区别

​	route是“路由信息对象”，包括path,params,hash,query,fullPath,matched,name等路由信息参数.

​	而router是“路由实例”包括了路由的跳转方法，钩子函数等、

## 26、Vue.js的两个核心是什么？

​	数据驱动，组件系统

## 27、vue常用的修饰符

​	**.stop** 阻止事件冒泡,使用了.stop后，点击子节点不会捕获到父节点的事件

​	**.prevent**	用于取消默认事件

​	**.capture**	与事件冒泡的方向相反，事件捕获由外到内,捕获事件：嵌套两三层父子关系，然后所有都有点击事件，点击子节点，就会触发从外至内 父节点-》子节点的点击事件

​	**.once**只执行一次，如果我们在@click事件上添加.once修饰符，只要点击按钮只会执行一次。

​	**.number** 	将输出字符串转为Number类型·（虽然type类型定义了是number类型，但是如果输入字符串，输出的是string）

​	**.trim**	自动过滤用户输入的首尾空格

## 28、http状态码301 302的区别，304是啥

**200 OK**是见得最多的成功状态码

**301 redirect: 301** 代表永久性转移(Permanently Moved)
​**302 redirect: 302** 代表暂时性转移(Temporarily Moved )

**304 Not Modified**: 当协商缓存命中时会返回这个状态码

401		请求的格式不对

**403 Forbidden**: 这实际上并不是请求报文出错，而是服务器禁止访问

**404 Not Found**: 资源未找到

**500 Internal Server Error**: 仅仅告诉你服务器出错了

**503 Service Unavailable**: 表示服务器当前很忙，暂时无法响应服务。

## 29、强缓存和协商缓存

强缓存：直接使用本地的缓存，不用跟服务器通信

协商缓存：将资源一些相关信息返回服务器，让服务器判断浏览器是否能直接使用 本地缓存，整个过程至少与服务器通信一次

## 30、事件委托 、 优缺点

​	事件委托是利用事件冒泡原理，让节点的父级代为执行事件。而不需要循环遍历元素的子节点，大大减少dom操作；
缺点：
1.不适应所有的事件，只适用于支持事件冒泡的事件
2.原理上执行就近委托



## 31、前端工程化思想

https://www.jianshu.com/p/88ed70476adb

​	前端工程化是使用软件工程的技术和方法来进行前端的开发流程、技术、工具、经验等规范化、标准化，其主要目的为了提高效率和降低成本，即提高开发过程中的开发效率，减少不必要的重复工作时间，而前端工程本质上是软件工程的一种，因此我们应该从软件工程的角度来研究前端工程。
​	前端工程化就是为了让前端开发能够“自成体系”，个人认为主要应该从**模块化、组件化、规范化、自动化**四个方面思考。
​	**1、模块化**
​	简单来说，模块化就是将一个大文件拆分成相互依赖的小文件，再进行统一的拼装和加载。
​	JS的模块化
​	在ES6之前，JavaScript一直没有模块系统，这对开发大型复杂的前端工程造成了巨大的障碍。对此社区制定了一些模块加载方案，如CommonJS、AMD和CMD等。
​	css的模块化
​	虽然SASS、LESS、Stylus等预处理器实现了CSS的文件拆分，但没有解决CSS模块化的一个重要问题：选择器的全局污染问题。

​	资源的模块化
Webpack的强大之处不仅仅在于它统一了JS的各种模块系统，取代了Browserify、RequireJS、SeaJS的工作。更重要的是它的万能模块加载理念，即所有的资源都可以且也应该模块化。
依赖关系单一化。所有CSS和图片等资源的依赖关系统一走JS路线，无需额外处理CSS预处理器的依赖关系，也不需处理代码迁移时的图片合并、字体图片等路径问题；
资源处理集成化。现在可以用loader对各种资源做各种事情，比如复杂的vue-loader等等；
项目结构清晰化。使用Webpack后，你的项目结构总可以表示成这样的函数： dest = webpack(src, config)。

**2、组件化**
从UI拆分下来的每个包含模板(HTML)+样式(CSS)+逻辑(JS)功能完备的结构单元，我们称之为组件。

组件化≠模块化。模块化只是在文件层面上，对代码或资源的拆分；而组件化是在设计层面上，对UI（用户界面）的拆分。

其实，组件化更重要是一种分治思想。

**3、规范化**
规范化其实是工程化中很重要的一个部分，项目初期规范制定的好坏会直接影响到后期的开发质量。

目录结构的规范
编码规范
前后端接口规范

**4、自动化**
前端工程化的很多脏活累活都应该交给自动化工具完成。任何简单机械的重复劳动都应该让机器完成
图标合并
持续继承
自动化构建
自动化部署
自动化测试

## 32、delete与vue.delete的区别

delete只是被删除的元素变成了 empty/undefined 其他的元素的键值还是不变。
Vue.delete 直接删除了数组 改变了数组的键值。
	

## 33、设立"严格模式"的优点：

1. 消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为;
2. 消除代码运行的一些不安全之处，保证代码运行的安全；
3. 提高编译器效率，增加运行速度；
4. 为未来新版本的Javascript做好铺垫。

## 34、类型强制Coercion

是将值从一种类型转换为另一种类型的过程（例如字符串转换为数字，对象转换为布尔值等）。任何类型，无论是原始类型还是对象，都是类型强制的有效主体
　 coercion有两种表现形式：显式和隐式。

```
js中的强制类型转换
```

转换为数字方法一：Number（）
方法二：parseInt()
方法三：parseFloat()
方法四：加号运算符
转换为字符串方法一：toString()
方法二：String()
方法四：加号运算符
转换为布尔值方法一：Boolean()
方法二：逻辑运算符！



## 36、性能优化

### 一.css优化

1.打包css文件

2.易维护：少用ID， !important，多用class

3.样式用外部样式，最好不要用行间样式，内嵌样式

4.选择器的层级最好不要超过4层，减少层级可减少渲染速度

5.可读性：类名的命名规范

6.可扩展性：css的整体设计，公用的样式抽取，减少冗余的，重复的样式

7.样式的引入放在头部

### 二.js优化

1.打包js

2.减少全局变量，全局方法的定义

3.减少闭包的使用，避免多层循环的嵌套

4.减少dom节点的事件绑定

5.删除多余的代码，公用方法的抽取

6.减少http请求次数

7.js的引用放在底部

8.避免重写，重绘次数

9.行为与页面分离：js最好写在外部文件

10.按需加载,js的延迟加载 deffer

### 三.h5的优化

1.减少多余的dom节点嵌套

2.标签的语义化使用，比如标题就用h1-h6，图文列表用figure figcaption，头部用 header，底部footer，导航nav，侧边菜单栏 aside，文章用article，模块用section等

3.使用数据缓存，sessionStorage，localStorage，离线缓存，indexedDB本地数据库

4.页面SEO的优化：title、keyword、description，图片的alt，a标签的title

### 四.图片的优化

1.减小图片的的大小，小图标使用svg,png，背景图片用jpg

2.雪碧图的使用，减少对服务器的请求次数

3.图片预加载

4.字体图标的使用（阿里巴巴字体图标库IconFont）

### 五.用户体验的优化

1.加载页面，请求接口的loading

2.页面的平滑滚动，颜色的渐变，适当的动画

3.减少操作次数，减少表单输入

4.优化页面加载的速度，缓存的合理使用，预加载的使用

5.操作图标的易读性

## 37、$.nextTick

异步操作dom

应用场景

下面了解下nextTick的主要应用的场景及原因。

- 在Vue生命周期的created()钩子函数进行的DOM操作一定要放在Vue.nextTick()的回调函数中

在created()钩子函数执行的时候DOM 其实并未进行任何渲染，而此时进行DOM操作无异于徒劳，所以此处一定要将DOM操作的js代码放进Vue.nextTick()的回调函数中。与之对应的就是mounted()钩子函数，因为该钩子函数执行时所有的DOM挂载和渲染都已完成，此时在该钩子函数中进行任何DOM操作都不会有问题 。

- 在数据变化后要执行的某个操作，而这个操作需要使用随数据改变而改变的DOM结构的时候，这个操作都应该放进Vue.nextTick()的回调函数中。

Vue.nextTick用于延迟执行一段代码，它接受2个参数（回调函数和执行回调函数的上下文环境），如果没有提供回调函数，那么将返回promise对象。

## 38、**vue2.0和vue3.0的区别**

vue2.几版本之前都是使用object.defineProperty来实现双向数据绑定的

而在vue3.0中这个方法被取代了

1. 为什么要替换Object.defineProperty

我感觉 替换不是因为不好，是因为有更好的方法使用效率更高

Object.defineProperty的缺点：

1.在Vue中，Object.defineProperty

无法监控到数组下标的变化，

导致直接通过数组的下标给数组设置值，不能实时响应。

2. Object.defineProperty只能劫持对象的属性,因此我们需要对每个对象的每个属性进行遍历。

Vue里，是通过递归以及遍历data对象来实现对数据的监控的，

如果属性值也是对象那么需要深度遍历,显然如果能劫持一个完整的对象，不管是对操作性还是性能都会有一个很大的提升。

而要取代它的Proxy有以下两个优点：

1. 可以劫持整个对象，并返回一个新对象

2. 有13种劫持操作

**vue3.0有了解吗？3.0在双向数据绑定方面有什么优化？**--改用了proxy

vue3.0

改用proxy为双向数据绑定原理，有了解没?

 什么是Proxy？

Proxy是 ES6 中新增的一个特性，翻译过来意思是"代理"，用在这里表示由它来“代理”某些操作。 **Proxy 让我们能够以简洁易懂的方式控制外部对对象的访问**。

其功能非常类似于设计模式中的代理模式。

Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。

## 39、图片上传原理

图片是静态资源，计算机无法识别，以流的形式上传至服务器，服务器解析流，以字符串的形式返回一个链接地址，

## 40、浏览器渲染

解析html以构建dom树 -> 构建render树 -> 布局render树 -> 绘制render树

![](/img/2021/2011110316263715.png)



## 41、Event.currentTarget

Event.target：返回触发事件的元素；

Event.currentTarget：返回绑定事件的元素。

## 42、封装 vue 组件的过程?

- 首先，组件可以提升整个项目的开发效率。能够把页面抽象成多个相对独立的模块，解决了我们传统项目开 发：效率低、难维护、复用性等问题。

- 然后，使用 Vue.extend 方法创建一个组件，然后使用 Vue.component 方法注册组件。子组件需要数据，可以 在 props 中接受定义。而子组件修改好数据后，想把数据传递给父组件。可以采用 emit 方法


## 43.重绘和重排（重流）的关系

**回流必将引起重绘，而重绘不一定会引起回流**

- 重绘：当渲染树中的元素外观（如：颜色）发生改变，不影响布局时，产生重绘
- 回流：当渲染树中的元素的布局（如：尺寸、位置、隐藏/状态状态）发生改变时，产生重绘回流

**如何最小化重绘(repaint)和回流(reflow)？**
需要要对元素进行复杂的操作时，可以先隐藏(display:"none")，操作完成后再显示
需要创建多个 DOM 节点时，使用 DocumentFragment 创建完后一次性的加入 document
缓存 Layout 属性值，如：var left = elem.offsetLeft; 这样，多次使用 left 只产生一次回流
尽量避免用 table 布局（table 元素一旦触发回流就会导致 table 里所有的其它元素回流）
避免使用 css 表达式(expression)，因为每次调用都会重新计算值（包括加载页面）,而且，css表达式是IE独有的，不建议使用
尽量使用 css 属性简写，如：用 border 代替 border-width, border-style, border-color 批量修改元素样式：elem.className 和 elem.style.cssText 代替 elem.style.xxx

## 44.js的内置函数有哪些?

```
数学函数:
sin方法	返回一个数的正弦值
cos方法	返回一个数的余弦值
random方法	返回一个0-1之间的随机数
parseInt方法	返回从字符串转换过来的整数
```



```
字符串函数:
charAt 方法 返回位于指定索引位置的字符
charCodeAt方法  返回指定字符的Unicode编码
concat 方法（Array） 返回一个由两个数组合并组成的新数组。
```

```
日期函数:
getDate 方法 使用当地时间返回 Date 对象的月份日期值。
getDay 方法 使用当地时间返回 Date 对象的星期几。
```

## 45.AMD和CMD是什么?

AMD:
异步模块加载。它是一个在浏览器端模块化开发的规范

特点:
适合在浏览器环境中异步加载模块。可以并行加载多个模块。
缺点：
提高了开发成本，并且不能按需加载，而是必须提前加载所有的依赖。

CMD:
1.对于依赖的模块AMD是提前执行，CMD是延迟执行。不过RequireJS从2.0开始，也改成可以延迟执行（根据写法不同，处理方式不通过）。

2.AMD推崇依赖前置（在定义模块的时候就要声明其依赖的模块），CMD推崇依赖就近（只有在用到某个模块的时候再去require——按需加载）。



## 46.父子组件渲染的过程是什么?

父组件的beforcreated先进行,created,beforMounted在进行子组件中的所有生命周期,再进行父组件的mounted再往下执行



## 47.什么是BFC？

BFC 全称为 块格式化

里面不影响外面的,外面的也不影响里面的.

**BFC 特性(功能)**

1. 使 BFC 内部浮动元素不会到处乱跑；
2. 和浮动元素产生边界。

```
触发条件:

浮动,

display

overflow

 display: table-cell
```

## 48.继承

（1）工厂模式：因为使用用一个接口创建很多对象会产生大量的重复代码,为了解决这个问题，人们就开始使用工厂模式。
（2）构造函数模式：也叫经典继承或伪造对象继承，使用构造函数模式我们能少些更多代码
（3）原型式继承，这种方法没有严格意义上的构造函数,想法是基于已有的对象来创建新对象,同时还不必创建自定义类型.
（4）组合继承也叫伪经典继承，指的是将原型链和借用构造函数的技术组合到一块,发挥二者之长的一种继承模式.其背后的思路是,通过原型链实现对原型属性和方法的继承,通过借用构造函数实现对实例属性的继承.这样就即实现了函数的复用,也保证了每个实例都有自己的属性.
（5）寄生式继承，寄生式继承的思路与寄生构造函数类似,即创建一个仅用于封装继承过程的函数,该函数在内部以某种方式来增强对象,最后在返回对象.
（6）组合寄生式继承，组合继承最大的问题就是无论什么情况下,都会调用两次超类型的构造函数:一次是在创建子类原型的时候,另一次是在子类构造函数的内部。所谓寄生组合式继承,即通过借用构造函数来继承属性,通过原型链的混成形式来继承方法.其基本思路是:不必为了指定子类型的原型而调用超类型的构造函数,我们所需要的只是超类型的一个副本而已.本质上就是使用寄生式继承来继承超类型的原型,然后再将结果指定给子类型的原型.

## 49.uni app用过没有？

uni-app，开发一次全端覆盖（iOS、Android、H5、微信小程序、百度小程序、支付宝小程序）

跨平台更多；（一套代码，多端发行；优雅的在一个项目里调用不同平台的特色功能！）
运行体验更好；（组件，api与微信小程序一致；兼容weex原生渲染）
通用技术栈，学习成本更低；（vue的语法，微信小程序的api内嵌mpvue）
开放生态，组件更丰富；
   -支持通过npm安装第三方包；

   -支持微信小程序自定义组件及SDK

   -兼容mpvue组件及项目

   -App端支持和原生混合编码

   -DCloud拥有插件市场

它类似于vue，微信小程序，这些前端的框架有很多的，公司用哪一个框架就学哪一个呗，坎级下基本很快就能上手的。

## 50,cookie和 sessionStorage和 localStorage的区别

**回答问题的思路：**

**共同点：**

都保存在浏览器端，且是同源的（顺便解释一下同源：域名、协议、端口号相同）

**不同点：**

**存储大小不同：**

cookie大小一般4k，sessionStorage一般5M，localStorage一般为5M

**有效期不同：**

cookie:设置有效期之前有效，当超过有效期之后会失效。 localstorage:永久有效，除非你进行手动删除

sessionStorage:当前会话有效，关闭浏览器会失效

**sessionStorage和localStorage的作用域的区别详情：**

不同浏览器无法共享localStorage和sessionStorage中的信息。

相同浏览器的不同页面间可以共享相同的localStorage（页面属于相同域名和端口），但是不同页面或标签页面无法共享sessionStorage的信息。

**带入项目：**

window.location.href跳转页面的时候会丢失cookie,不是其他的什么原因，而是需要设置cookie作用域的路径path

localhost使用出现的问题：

localStorage和sessionStorage兼容到ie8，判断浏览器是否支持：

```
if（window.localStorage）{
    alert("浏览器支持localStorage")
}Copy
```

存：localStorage.setItem(“arr”,JSON.stringify(data.body.data)),需要转换成字符串，不能直接存对象，切记

取：JSON.parse(localStorage.arr)



## 滴滴面试题

### 1，执行顺序：

```js
async function async1(){
	console.log('async1 start')//2
	await async2()
	console.log('async1 end')//6
}
async function async2(){
	console.log('async2')//3
}
console.log('script start')//1
setTimeout(function() {
	console.log('setTimeout')//8
},0)
async1()
new Promise(function(resolve) {
	console.log('promise1')//4
	resolve()
}).then(function(){
	console.log('promise2')//7
})
console.log('script end')//5
```

### 2,常见的数据类型

基本数据类型：undefined，string，number，boolean,null

引用数据类型：obj，array,function

### 3，闭包的理解，用途及注意事项：

函数执行时创建了一个内部函数，这个内部函数作为返回值，或以某种方式保留下来（属性），之后才会调用，这就会形成了闭包。

特点：

1，一个闭包就是当一个函数返回时，一个没有释放资源的栈区。

2，作为一个函数变量的一个引用，当函数返回时，其处于激活状态。

场景：

1，匿名自执行函数

我们创建了一个匿名的函数，并立即执行它，**由于外部无法引用它内部的变量，因此在函数执行完后会立刻释放资源，关键是不污染全局对象**。

```
(function() {
var days = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'],
today = new Date(),
msg = 'Today is ' + days[today.getDay()] + ', ' + today.getDate();
alert(msg);
} ());
```

2，结果缓存

我们开发中会碰到很多情况，设想我们有一个处理过程很耗时的函数对象，每次调用都会花费很长时间，那么我们就需要将计算出来的值存储起来，当调用这个函数的时候，首先在缓存中查找，如果找不到，则进行计算，然后更新缓存并返回值，如果找到了，直接返回查找到的值即可。**闭包正是可以做到这一点，因为它不会释放外部的引用，从而函数内部的值可以得以保留。**

优点
 1.可以读取函数内部的变量
 2.可以让这些局部变量保存在内存中，实现变量数据共享。

缺点
 1.由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。
 2.闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。

https://www.cnblogs.com/xuniannian/p/7452086.html

### 4,垃圾回收机制:

https://www.jianshu.com/p/23f8249886c6

### 5,协商缓存,强缓存

https://www.jianshu.com/p/9c95db596df5

### 6,微任务,宏任务:

### 7,浏览器渲染过程:

### 8,es6新增:

https://www.jianshu.com/p/0120580f39aa

### 9,怎么封装组件的:

### 10,打包:



















# 小程序总结

## 1.小程序之间的组件传值？

```
第一种：全局传值

// 步骤一：在全局app.js文件中定义数据
App({
  globalData: {
    userInfo: null,
    userName: '全局变量传值',
  }
})

// 步骤二：获取应用实例，不然无法调用全局变量
const app = getApp()

// 步骤三：调用全局变量
Page({
  data: {
  
  },
  onLoad: function (options) {
    console.log(app.globalData.userName);
  },
})

```



```
第二种：url传值

// 步骤一：使用关键字bindtap绑定一个点击事件方法，data-index是传入一个值
<image class="btn-detail" src='/images/btn_detail.png' bindtap='toDetail' data-index='{{index}}'></image>

// 步骤二：在脚本文件中定义这个方法（方法并不是定义在一个methods集合中的）
Page({
  data: {},
  onLoad: function () {},
  toDetail: function(e){
    // index代表的遍历对象的下标
    var index = e.currentTarget.dataset.index;
    var proList = this.data.proList;
    var title = proList[index].proName;
    wx.navigateTo({
      url: '/pages/detail/detail?title='+title,
    })
  }
})

// 步骤三：在detail组件的脚本文件中接收title这个传入过来的值
Page({
  data: {},
  onLoad: function (options) {
    console.log(options.title);
  },
})


```



```
第三种：Storage传值

// 步骤一：使用关键字bindtap绑定一个点击事件方法，data-index是传入一个值
<image class="btn-detail" src='/images/btn_detail.png' bindtap='toDetail' data-index='{{index}}'></image>

// 步骤二：在脚本文件中定义这个方法（方法并不是定义在一个methods集合中的）
Page({
  data: {},
  onLoad: function () {},
  toDetail: function(e){
    var index = e.currentTarget.dataset.index;
    var proList = this.data.proList;
    var title = proList[index].proName;
    wx.setStorageSync('titleName', title);
    wx.navigateTo({
      url: '/pages/detail/detail',
    })
  }
})

// 步骤三：在detail组件的脚本文件中使用wx.getStorageSync接收titleName这个传入过来的值
Page({
  data: {},
  onLoad: function (options) {
    var titlen = wx.getStorageSync('titleName');
    console.log(titlen);
  },
})

```

## 2，小程序跳转的api？

一、wx.navigateTo(obj)
特点：
可以传值，新页面获取值用option.name
跳抓到新页面后有返回按钮 可以返回上一个页面

wx.navigateTo({
     url: '/pages/Deposit/Deposit?merchantId=' + this.data.coach.coachId,
})

二、wx.redirectTo(obj)
特点：
可以传值，新页面获取值用option.name
跳抓到新页面后无返回按钮 不能返回上一个页面

wx.redirectTo({
        url: '/pages/Deposit/Deposit?merchantId=' + this.data.coach.coachId,
})

三、wx.reLaunch(obj)
特点：
可以传值，新页面获取值用option.name
如果跳转的页面路径是 tabBar 页面则不能带参数
跳抓到新页面后无返回按钮 不能返回上一个页面

wx.reLaunch({
        url: '/pages/Deposit/Deposit?merchantId=' + this.data.coach.coachId,
})

四、wx.switchTab(obj)
特点：
地址后面不可以带参数
跳转的 tabBar 页面的路径 需在 app.json 的 tabBar 字段定义的页面
跳抓到新页面后无返回按钮 不能返回上一个页面

wx.switchTab({
        url: '/index'
})

五、navigator标签的url跳转页面
六、wx.navigateBack(obj)
特点：
关闭当前页面，返回上一页面或多级页面。
可通过 getCurrentPages() 获取当前的页面栈，决定需要返回几层。
参数：
delta number类型 返回的页面数，如果 delta 大于现有页面数，则返回到首页

wx.navigateBack({
        delta: 2
})

# html,css

## 动画

```
动画animation:
	动画与过渡不同，它可以自动执行
	动画创建的步骤:
		1. 绑定一个选择器
		动画执行的两个必要条件：
			动画的名称：
			动画的过渡时间：
		2. 遵循@keyframes aniName{
				from...to 
				或
				百分比（建议）
				}
动画属性的介绍：
	animation-name        动画的名称
	animation-duration    动画的过渡时间
	animation-delay       动画的延迟过渡时间
	animation-timing-function  动画的作用曲线，过渡的速率
	animation-direction        动画的播放顺序
	normal(默认，正向播放)
	reverse(反向播放)
	alternate(奇数次正向播放，偶数次反向播放)
	alternate-reverse(偶数次正向播放，奇数次反向播放)	
	animation-iteration-count  动画的播放次数
		默认播放一次，如果想要无限次播放，需要设置属性值为 			infinate
```

## 过度

```
过渡transition：
	从一种效果过渡到另外一种效果，需要触发条件才能实现过渡，如：鼠标滑过开始过渡
	除了触发条件外，过渡的实现还需要两个必要条件：
	过渡的属性：transition-property 
	过渡的时间：transition-duration
过渡的属性如下：
	transition-property         过渡的属性
	transition-duration         过渡的时间
	transition-delay            过渡的延迟时间
	transition-timing-function  过渡的作用曲线，过渡的速率
     过渡需要条件触发，目前使用hover
设置3d变形
transform-style:flat(2d) | preseve-3d;
transform-origin:bottom | left right | center l top...;
perspective设置透视度
```

## 变形

```
变形transform：
	平移：translate(x,y)    translate3D(x,y,z)
 translateX0	 tramslateY0
	缩放：scale(x,y)	scaleX0	scaleY0
	旋转：rotate()    rotate3D()	rotateX0	rotateY0	rotateZ0
	单位： deg(度)  turn(圈)
	倾斜：skew()	skewX0	skewY( 
	矩阵：martrix()
	设置中心原点：
	transform-origin     属性值一般跟方向值即可、百分比也行
	设置2d还是3d变形：
		transform-style:preserve-3d(3D变形) | flat(默认，2d变形);
	如果设置3d变形，还需要设置一定的透视度，才能达到相应的效果：
	perspective:像素值;
	注意：3d变形与透视度需要在父元素中设置，在子元素中生效
			1.变形与过渡
```

## felx布局

回答思路：

1、flex的父元素的六个属性：

 ①display => flex;

 ②flex-direction => 决定主轴的方向，它包括row, row-reverse, column, column四个值，默认是row，从左向右排列元素

 ③flex-wrap => 它有三个值：wrap（空间不足时换行），nowrap（空间不足时也换行，会挤在一起,这也是默认值），wrap-reverse(换行，但是第一行在下面)

 ④flex-flow =>flex-direction 和flex-wrap的简写，属性是【flex-direction，flex-wrap】的属性值

 ⑤justify-content => flex-start 、 flex-end 、 center 、 space-between 、 space-around

 ⑥align-items => flex-start 、 flex-end 、 center 、baseline 、 stretch

项目元素（子元素）的六个属性：

- `order`
- `flex-grow`
- `flex-shrink`
- `flex-basis`
- `flex`
- `align-self`

2、工作中写flex布局遇到的问题：

再写页面的时候,一行四个排列,调整flex布局,一般都是左右space-between,自动排列,最后一行如果剩余两个元素,就会左右两端各一个,这样就会不符合排列的规则;

解决方案:

