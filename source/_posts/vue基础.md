---
title: vue基础
date: 2020-09-16 20:15:04
tags: vue
categories: 知识点总结
---

# vue基础

## vue01

### 快速学习一个框架的方法

封装js代码  框架实质

2遍

对着文档撸一遍

用项目提升自己的框架能力

从新看一遍文档，更深层次对框架有自己的理解

### 体验vue

```
var vm=new Vue({
	el:"#app",
	data:{
		msg:"hello vue"
	}
})
```

### vue表达式

```
msg:"hello world",
htmlStr:"<p>我是一个p标签</p>",
number:0,
ok:false,
arrStr:"a,b,c,d"
```

```
<!-- 插值表达式 -->
{{msg}}
<br/>
<!-- 标签能否按照原样输出 -->
{{htmlStr}}
<br/>
<!-- js运算表达式 -->
{{number+1}}
<br/>
<!-- bol -->
{{ok?"yes":"no"}}
<br/>
<!-- 字符串的方法 -->
{{arrStr.split(",")}}
```

### v-html和v-text

```
strHtml:"<p>因为爱情</p>",

xss:"<p onclick='alert(1)'>哇哈哈</p>"
```

view

```
<div v-html="strHtml"></div><div v-html="xss"></div>
```

v-text

```
strHtml:"<p>因为爱情</p>"
v-text="strHtml"
```

### v-bind

```
src:"./4.jpg",

alt:"美女图片",

id:"1"
```

```
v-bind:src="src" v-bind:alt="alt" v-bind:id="id"
```

### v-style

```
isPink:true,

isFont:true,

color:"pink",

fontSize:"font"
```

```
v-bind:class="{pink:isPink,font:isFont}"

v-bind:class="[color,fontSize]"
```

```
.pink{
	color: pink;
}
.font{
	font-size: 20px;
}
```

### v-bind:style

```
color:"pink",
fontSize:20,
myColor:{
color:"pink"
},
myFontSize:{
fontSize:"20px"
}
```

```
v-bind:style="{color:color,fontSize:fontSize+'px'}"

v-bind:style="[myColor,myFontSize]"
```

### v-bind缩写

```
:src="src" :alt="alt"
```

### 单向数据绑定

```
msg:"hello world"      {{msg}}
```

### v-model双向数据绑定

```
msg:"hello world"       <input type="text" v-model="msg">
```

### v-on

```
go(){
 	alert("发生了什么");
}
```

```
<p v-on:click="go">点我一下</p> 
缩写 
<p @click="go">再点我一下</p> 
带默认事件的方法 
<a href="http://www.baidu.com" @click.prevent="go">哈哈哈</a>
```

### v-for

```
arr:["a","b","c"],
obj:{
"name":"123",
"age":18,
"sex":"女"
}
```

### v-show

```
v-show="isShow"
```

### v-if

```
v-if="isShow"
```

### 单选框

```
男: <input type="radio" name="sex" value="男" v-model="sex">
女: <input type="radio" name="sex" value="女" v-model="sex">
sex:"男"
```

### 下拉框

```
<select name="" id="" v-model="selected">
<option value="-1">请选择您想呆的城市</option>
<option value="0">北京</option>
<option value="1">上海</option>
<option value="2">深圳</option>
</select>

selected:-1
```

## vue02

### 全选反选

```
<!DOCTYPE html>

<html>



	<head>

		<meta charset="UTF-8">

		<title></title>

	</head>



	<body>

		<div id="app">

			<button @click="checkAnti">反选</button>

			<button @click="checkAll">全选</button>

			<button @click="checkNone">全不选</button> 喜好:

			<div v-for="item in inputArr">

				{{item.text}} : <input type="checkbox" value="" v-model="item.checked">

			</div>

		</div>

	</body>

	<script src="vue.js" type="text/javascript" charset="utf-8"></script>

	<script type="text/javascript">

		new Vue({

			el: "#app",

			data: {



				inputArr: [{

						text: '足球',

						checked: true

					},

					{

						text: '篮球',

						checked: false

					},

					{

						text: '羽毛球',

						checked: false

					},

					{

						text: '游泳',

						checked: false

					},

				],



			},

			methods: {

				checkNone() {

					this.inputArr.forEach(item => {

						item.checked = false;

					});

				},

				checkAll() {

					this.inputArr.forEach(item => {

						item.checked = true;

					});

				},

				checkAnti() {

					this.inputArr.forEach(item => {

						item.checked = !item.checked;

					});

				}

			}

		})

	</script>



</html>
```



### 事件类型

```
methods:{
    doThis(){
     alert("我就是惊喜");
    }
}
<button @click="doThis">点我有惊喜</button>
<input type="text" @keyup="doThis">
```

### 阻止浏览器默认事件儿或者事件儿冒泡

```
methods:{
 doThis(ev){
        ev.preventDefault();
        console.log("我跳还是不跳");
    }
}
<a href="http://www.baidu.com" @click="doThis">跳到百度</a>
```

### 事件参数

```
methods:{
doThis(id,e){
 	e.preventDefault();
	console.log(id)
	}
}

<!-- 假如普通参数和事件对象放在一起的时候，记得要用$event 代表事件对象 -->
<a href="http://www.souhu.com" @click="doThis(1,$event)">搜狐</a> 
```

### 私有过滤器

```
 //hello world反过来  app的私有过滤器
filters:{//过滤器
 	str3(st4r){
	return str.slice(0,3)
},
myReverse(val){
 	return val.split("").reverse().join("");
	}
}
<div id="app">
    {{msg|str3}}
    {{msg|myReverse}}
</div>
<div id="app2">
	{{vue|myReverse}}
</div>
```

### 全局过滤器

```
Vue.filter("myReverse",(val)=>{
 	return val.split("").reverse().join("");
)
```

### 过滤器案例

```
 Vue.filter("myTime",(val,conone,contwo)=>{
//获取年份
let y=val.getFullYear();
let m=val.getMonth()+1;
let d=val.getDate();
     // 2019-- 3-- 8  模版字符串
     return `${y}${conone}${contwo}${m}${conone}${contwo}${d}`;
})
var vm=new Vue({
el:"#app",
 data:{
	msg:new Date()
 }
})
<!-- 函数调用 -->
{{msg|myTime("-","-")}}
```

### 自定义局部指令

```
directives:{
"myfocus":{
 	//追加 inserted 
	inserted:function(el){
 		el.focus()
 	}
	} 
 }
 <!-- 添加指令:v-自定义指令名词 -->
 <input type="text" id="id1" v-myfocus/>
```

### 自定义全局指令

```
Vue.directive("myfocus",{
 "inserted":(el)=>{
	el.focus();
  }
})
<div id="app">
<input type="text" v-myfocus>
</div>
<div id="app2">
<input type="text" v-myfocus>
</div>
```

### 自定义指令的问题

```
Vue.directive('myfocus',{
    "inserted":function(el){
    	el.focus()  //focus属性绑定了最后一个
     el.style.background="pink"  
     }
})
var vm=new Vue({
    el:"#app",
    methods:{
    	"aa":function(){
     }
    },
    directives:{
     	"myfocuss":{
    	 inserted:function(el){
     		el.focus()
     	}
      }
    }
})
<input type="text" v-myfocus>
<input type="text" v-myfocus>
<input type="text" v-myfocus>
<input type="text" v-myfocus>
<input type="text" v-myfocus>

<input type="text" v-myfocuss>
```

### 购物车案例

```
<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <meta http-equiv="X-UA-Compatible" content="ie=edge">

    <title>Document</title>

    <style>

        \#shop{

            width: 500px;

            margin: 40px auto;

            text-align: center;

        }

        table{

            border-collapse: collapse;

            width: 500px;

            margin: 40px auto;

        }

        th,td{

            padding: 6px 0;

            text-align: center;

        }

        span{

            cursor: pointer;

        }

        span:hover{

            color: red;

        }

        input{

            width: 30px;

            border: none;

            text-align: center;

        }

    </style>

</head>

<body>

    <div id="app">

        <table border="1">

            <thead>

                <thead>

                    <tr>

                        <th>商品名称</th>

                        <th>单价</th>

                        <th>操作</th>

                    </tr>

                </thead>

                <tbody>

                    <tr v-for = "i of arr.entries()">

                        <td>{{i[1].name}}</td>

                        <td>{{i[1].price}}</td>

                        <td><span @click="add(i[0])">购买</span></td>

                    </tr>

                </tbody>

            </thead>

        </table>

        <div id="shop">

            <h2>购物车</h2>

            <table border="1"> 

                <thead>

                    <tr>

                        <th>购买商品</th>

                        <th>数量</th>

                        <th>价格</th>

                    </tr>

                </thead>

                <tbody>

                    <!-- [0,{na...}] -->

                        <tr v-for = "i of shopArr.entries()">

                            <td v-text= "i[1].name"></td>

                            <td><button @click = "readnum(i[0])">-</button><span v-text= "i[1].count"></span><button @click = "addnum(i[0])">+</button></td>

                           <td v-text= "i[1].count*i[1].price"></td>

                        </tr>

                </tbody>

            </table>

            <p>总价格：<strong v-text = "zzj"></strong></p>

        </div>

    </div>

</body>

<script src="./vue.js"></script>

<script>

    var vm = new Vue({

        el:"#app",

        data:{

            //购买

            arr:[{name:"白菜",price:10},{name:"芹菜",price:20},{name:"香菜",price:30}],

            //购物车

            shopArr:[],

            //总计

            zzj:0,

        },

        methods:{

             add(i){

                 //计数

                let num = 0;

                for(var j = 0;j<this.shopArr.length;j++){

                    if(this.shopArr[j].name == this.arr[i].name){

                        num++;

                        this.shopArr[j].count++;

                    }

                }

                if(num == 0){

                    this.arr[i].count = 1;

                    this.shopArr.push(this.arr[i]);

                } 

                this.zj()

            },

            zj(){

                var shopNum = 0;

                for(var j = 0;j<this.shopArr.length;j++){

                    shopNum += this.shopArr[j].count * this.shopArr[j].price

                }

                this.zzj = shopNum;

            },

            addnum(i){

                this.shopArr[i].count++;

                this.zj()

            },

            readnum(i){

                this.shopArr[i].count--;

                if(this.shopArr[i].count == 0){

                    this.shopArr.splice(i,1)

                }

                this.zj()

            }

        }

    })

</script>

</html>
```

## vue03

### 初识组件儿

```
Vue.component("mycom",{
​        template:`

            <div>我是一个组件</div>

​            {{name}}

​        `,

​        data(){

​           return {

​               "name":"哇哈哈"

​           } 

​        }

​    })
```

### 局部组件儿

```
var mycom={
template:`
	<div>我是一个组件</div>
 }
 el:"#app",
 data:{},
 //局部组件
 components:{
 mycom
 }
```

### template

```
var vm=new Vue({

​        el:"#app",

​        template:`<div>我是模版字符串</div>`,

​        data:{

​            msg:"hello world"

​        }

​    })
```

### 全局组件儿分离

```
Vue.component("mycom",{

​        template:"#show"

​    })

​    var vm=new Vue({

​        el:"#app",

​        data:{}

​    })
<div id="app">
<mycom></mycom>
</div>
<!-- 这里注意:type属性不是script 变成了template -->
<script type="text/template" id="show">
<div class="show">
<p>哇哈哈</p>
</div>
</script>

获取全局组件的view层写法
注意事项:
全局组件:template:#show==>dom层放在了body中
script标签，一定要有id属性，跟你全局中定义的是一样的
你所有的dom必须包裹在class为show的div中，不然会报错
```

### 局部组件儿分离

```
 var mycom={

​        template:"#show",

​        data(){

​            return {

​                msg:"hello world"

​            }

​        }

​    }

​    var vm=new Vue({

​        el:'#app',

​        data:{},

​        components:{

​            mycom

​        }

​    })
<div id="app">
<mycom></mycom>
</div>
<!-- 局部组件的视图层view -->
<template id="show">
<div class="show">
<p>哇哈哈</p>
</div>
</template>
```

### 动态绑定组件:

```html
<div id="app">
            <div class="btn">
                <button type="button" v-for="(item,i) in arrBtns" @click="fn(i)" :class="{active:n===i}">
                    {{item}}
                </button>
            </div>
            <div class="component">
                <component :is="myCom"></component>
            </div>
</div>
```

```js
var Kky1={
				data(){
					return {
						str1:"这是甲的故事"
					}
				},
				template:`
					<div class="kky1">
						甲组件<br />
						<input type="text" v-model="str1" />
						<br />
						str1===>{{str1}}
					</div>
				`
			}
			var Kky2={
				data(){
					return {
						str2:"这是乙的故事"
					}
				},
				template:`
					<div class="kky2">
						乙组件<br />
						<input type="text" v-model="str2" />
						<br />
						str2===>{{str2}}
					</div>
				`
			}
			var Kky3={
				data(){
					return {
						str3:"这是丙的故事"
					}
				},
				template:`
					<div class="kky3">
						丙组件<br />
						<input type="text" v-model="str3" />
						<br />
						str3==>{{str3}}
					</div>
				`
			}
			var Kky4={
				data(){
					return {
						str4:"这是丁的故事"
					}
				},
				template:`
					<div class="kky4">
						丁组件<br />
						<input type="text" v-model="str4" />
						<br />
						str4===>{{str4}}
					</div>
				`
			}
			var app=new Vue({
				el:"#app",
				data:{
					arrBtns:["甲","乙","丙","丁"],
					comArr:["Kky1","Kky2","Kky3","Kky4"],
					n:0,
					myCom:"Kky1"
				},
				methods:{
					fn(i){
						this.n=i;
						this.myCom=this.comArr[i];
					}
				},
				components:{
					Kky1,
					Kky2,
					Kky3,
					Kky4
				},
            })
```

### 内置的组件:



### 缓存组件:keep-alive

keep-alive包含的会缓存输入的数据,

没有包含的不会缓存,

include="ak,ck",是指ak和ck会缓存,而其他的不会缓存,

exclude="bk,dk",代表着取反的意思.

两个生命周期:在使用过keep-alive之后才生效:

activated()该组件被激活时生效,

deactivated()该组件被移除是生效

```html
			<div class="component myBorder">
				无keep-alive缓存<br  />
				<component :is="myCom"></component>
			</div>
			<div class="component myBorder">
				keep-alive缓存<br  />
				<keep-alive>
					<component :is="myCom"></component>
				</keep-alive>
			</div>
			<div class="component myBorder">
				通过include  指定缓存组件 <br  />
				<keep-alive include="ak,ck">
					<component :is="myCom"></component>
				</keep-alive>
			</div>
			<div class="component myBorder">
				通过exclude  指定不缓存 组件<br  />
				<keep-alive exclude="bk,dk">
					<component :is="myCom"></component>
				</keep-alive>
			</div>
```

```js
var Kky1={
				name:"ak",
				data(){
					return {
						str1:"这是甲的故事"
					}
				},
				template:`
					<div class="kky1">
						甲组件<br />
						<input type="text" v-model="str1" />
						<br />
						str1===>{{str1}}
					</div>
				`,
				activated(){
					console.log("kky1 该组件被激活");
				},
				deactivated(){
					console.log("kky1 该组件被移除");
				}
			}
			var Kky2={
				name:"bk",
				data(){
					return {
						str2:"这是乙的故事"
					}
				},
				template:`
					<div class="kky2">
						乙组件<br />
						<input type="text" v-model="str2" />
						<br />
						str2===>{{str2}}
					</div>
				`,
				activated(){
					console.log("kky2 该组件被激活");
				},
				deactivated(){
					console.log("kky2 该组件被移除");
				}
			}
			var Kky3={
				name:"ck",
				data(){
					return {
						str3:"这是丙的故事"
					}
				},
				template:`
					<div class="kky3">
						丙组件<br />
						<input type="text" v-model="str3" />
						<br />
						str3==>{{str3}}
					</div>
				`,
				activated(){
					console.log("kky3 该组件被激活");
				},
				deactivated(){
					console.log("kky3 该组件被移除");
				}
			}
			var Kky4={
				name:"dk",
				data(){
					return {
						str4:"这是丁的故事"
					}
				},
				template:`
					<div class="kky4">
						丁组件<br />
						<input type="text" v-model="str4" />
						<br />
						str4===>{{str4}}
					</div>
				`,
				activated(){
					console.log("kky4 该组件被激活");
				},
				deactivated(){
					console.log("kky4 该组件被移除");
				}
			}
			var app=new Vue({
				el:"#app",
				data:{
					arrBtns:["甲","乙","丙","丁"],
					comArr:["Kky1","Kky2","Kky3","Kky4"],
					n:0,
					myCom:"Kky1"
				},
				methods:{
					fn(i){
						this.n=i;
						this.myCom=this.comArr[i];
					}
				},
				components:{
					Kky1,
					Kky2,
					Kky3,
					Kky4
				},
				computed:{

				},
			});
```

### 组件儿传值

```
Vue.component("mycom",{

​        template:'#show',

​        data(){

​            return {

​                msg1:"world"

​            }

​        },

​        props:["abc"]

​    })

​    //父传子

​    //组件之间的传值 通讯  数据的传递

​    var vm=new Vue({

​        el:"#app",

​        data:{

​            msg0:"hello"

​        }

​    })
<div id="app">
<mycom v-bind:abc="msg0"></mycom>
</div>
<!-- 分离的全局组件 -->
<!-- 子组件的view层 -->
<script type="text/template" id="show">
<div class="show">
{{abc}}{{msg1}}
</div>
</script>
```

### 父传子案例

```
Vue.component("mycom",{

​        template:"#show",

​        data(){

​            return {

​                msg3:"三大"

​            }

​        },

​        props:["msg0","msg1"]

​    })

​    //局部组件

​    var mycom1={

​        template:"#entire",

​        data(){

​            return {

​                msg4:"吃饭",

​                msg5:"睡觉"

​            }

​        },

​        props:["msg2"]

​    }

​    var vm=new Vue({

​        el:"#app",

​        data:{

​            msg0:"车萍",

​            msg1:"爱好",

​            msg2:"打豆豆"

​        },

​        components:{

​            mycom1

​        }

​    })
<div id="app">
<mycom :msg0="msg0" :msg1="msg1"></mycom>
<mycom1 :msg2="msg2"></mycom1>
</div>

<!-- 全局视图 -->
<script type="text/template" id="show">
<div class="show">
{{msg0}} {{msg3}} {{msg1}}
</div>
</script>
<!-- 局部试图 -->
<template id="entire">
<div class="entire">
{{msg4}} {{msg5}} {{msg2}}
</div>
</template>
```

### 子传父

```
var mycom={

​        template:"#show",

​        data(){

​            return {

​                cf:"吃饭",

​                sj:"睡觉"

​                //子组件中的数据

​            }

​        },

​        //追加一个业务，用于传值

​        methods:{

​            add(cf,sj){

​                //第一个参数 自定义的方法业务myevent 你要往父组件中传的值

​                this.$emit("myevent",cf,sj)//如果多个值，需要逗号隔开

​            }

​        }

​    }

​    //全局组件

​    Vue.component("mycom1",{

​        template:"#qj",

​        data(){

​            return {

​                sd:"三大"

​            }

​        },

​        methods:{

​            fs(sd){

​                //自定义方法  发送三大

​                this.$emit("fssd",sd)

​            }

​        }

​    })

​    //定义vue实例

​    var vm=new Vue({

​        el:"#app",

​        data:{

​            cf:"",

​            sj:"",

​            cp:"车萍",

​            ah:"爱好",

​            dd:"打豆豆",

​            sd:""

​        },

​        components:{

​            mycom

​        },

​        //子组件中有业务,并且有传值,我们要接收一下

​        methods:{

​            //接收业务 要把方法和你传过来的值都接收一下

​            fn(m,n){

​                this.cf = m;

​                this.sj = n;

​            },

​            //接收三大

​            fnn(s){

​                this.sd = s;

​            }

​        }

​    })
<div id="app">
{{cp}}{{sd}}{{ah}}<br/>
{{cf}}{{sj}}{{dd}}
<mycom v-on:myevent="fn"></mycom>
<mycom1 v-on:fssd="fnn"></mycom1>
</div>   
<!-- 局部试图层 -->
<template id="show">
<div class="show">
<!-- 调用方法 -->
{{add(cf,sj)}}
</div>
</template> 
<!-- 全局视图层 -->
<script type="text/template" id="qj" >
<div class = "qj">
{{fs(sd)}}
</div>
```

## vue04

### 基础路由

```
// 定义我们的路由组件

​    var movie={

​        template:"<div>我是电影界面</div>"

​    }

​    var music={

​        template:"<div>我是音乐界面</div>"

​    }

​    //定义路由，每个路由映射响应的组件

​    var routes=[

​        {path:"/movie",component:movie},

​        {path:"/music",component:music}

​        

​    ]

​    //创建路由实例，然后传入我们的routes配置

​    var router=new VueRouter({

​        // routes:routes

​        routes

​    })

​    //创建挂载 根实例

​    var vm=new Vue({

​        el:"#app",

​        data:{},

​        router

​    })
<!-- 第一步 -->
<div id="app">
<!-- 使用router-link来导航 -->
<!-- <a href=""></a> -->
<!-- 面试路由第一个问题
原理 -->
<router-link to="/movie">电影</router-link>
<router-link to="/music">音乐</router-link>
<!-- 进来第二步,设置路由出口 -->
<!-- 讲我们路由匹配的组件渲染到页面中 -->
<router-view></router-view>
</div>
```

### 配合组件儿写路由

```
// 定义你的组件  vue特点 组件化开发

​    var home={

​        template:"#home"

​    }

​    var movie={

​        template:"#movie"

​    }

​    var music={

​        template:"#music"

​    }

​    var we={

​        template:"#we"

​    }

​    //定义路由

​    var routes=[

​        {path:"/home",component:home},

​        {path:"/movie",component:movie},

​        {path:"/music",component:music},

​        {path:"/we",component:we},

​        {path:"*",redirect:"/home"}//设置路由初始界面

​    ]

​    //实例话你的路由

​    var router=new VueRouter({

​        routes

​    })

​    //挂载在根实例上

​    var vm=new Vue({

​        el:"#app",

​        data:{},

​        router

​    })
<div id="app">
<!--   to关键字跳转 -->
<router-link to="/home">首页</router-link>
<router-link to="/movie">电影</router-link>
<router-link to="/music">音乐</router-link>
<router-link to="/we">我们</router-link>

<!-- router-view 渲染页面 -->
<router-view></router-view> 
</div>

<!-- 局部组件我们怎么获取 -->
<template id="home">
<div class="home">
<div>我是home页面</div>
</div>
</template> 
<template id="movie">
<div class="movie">
<div>我是movie页面</div>
</div>
</template>
<template id="music">
<div class="music">
<div>我是music页面</div>
</div>
</template>
<template id="we">
<div class="we">
<div>我是we页面</div>
</div>
</template>
```

### 二级路由

```
// 定义组件

​    var home={

​        template:"#home"

​    }

​    var movie={

​        template:"#movie"

​    }

​    var music={

​        template:"#music"

​    }

​    var we={

​        template:"#we"

​    }

​    // 写两个子组件

​    var classic={

​        template:"#classic"

​    }

​    var popular={

​        template:"#popular"

​    }

​    //设置路由

​    var routes=[

​        {path:"/home",component:home},

​        {path:"/movie",component:movie},

​        {

​            path:"/music",component:music,

​            //设置二级路由

​            children:[

​                {path:"/classic",component:classic},

​                {path:"/popular",component:popular},

​                {path:"*",redirect:"/music"}

​            ]

​        },

​        {path:"/we",component:we},

​        //redirect 重定向 ／home  fn to -from watch

​        //设置页面初始化路由

​        {path:"*",redirect:"/home"}

​    ]

​    var router=new VueRouter({

​        routes

​    })

​    var vm=new Vue({

​        el:"#app",

​        data:{},

​        router

​    })
<div id="app">
<router-link to="/home">首页</router-link>
<router-link to="/movie">电影</router-link>
<router-link to="/music">音乐</router-link>
<router-link to="/we">关于我们</router-link>

<router-view></router-view>
</div>
<!-- 设置组件的内容 -->
<template id="home">
<div class="home">
home页面
</div>
</template>
<template id="movie">
<div class="movie">
movie页面
</div>
</template>
<template id="music">
<div class="music">
music页面
<router-link to="/classic">古典</router-link>
<router-link to="/popular">流行</router-link>

<router-view></router-view>
</div>
</template>
<template id="we">
<div class="we">
关于我们页面
</div>
</template>
<template id="classic">
<div class="classic">
古典
</div>
</template>
<template id="popular">
<div class="popular">
流行
</div>
</template>
```

### 命名路由

```
 // "/"  为了方便下午的传值 带入我们命名路由的知识点

​    var movie={

​        template:"#movie"

​    }

​    var music={

​        template:"#music"

​    }

​    var routes=[

​        {path:"/movie",component:movie},

​        {path:"/music",name:"music",component:music},

​    ]

​    var router=new VueRouter({

​        routes

​    })

​    var vm=new Vue({

​        el:"#app",

​        data:{},

​        router

​    })
<div id="app">
<!-- 普通路由的写法 -->
<router-link to="/movie">电影</router-link>
<!-- 命名路由的方法 -->
<router-link :to="{name:'music'}">音乐</router-link>
<!-- 渲染 -->
<router-view></router-view>
</div>
<template id="movie">
<div class="movie">
大人物
</div>
</template>
<template id="music">
<div class="music">
清风
</div>
</template>
```

### 模拟美团

```
<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <meta http-equiv="X-UA-Compatible" content="ie=edge">

​    <title>Document</title>

    <script src="vue.js"></script>

    <script src="vue-router.js"></script>

</head>

<body>

    <div id="app">

​       <router-view></router-view>

​    </div>

​    <template id="search">

        <div>

​            <input type="text" v-model="search">

​            <ul>

​                <li v-for="(item,index) in searchData" :key="index">

​                    <ul v-for="value in item.city">

​                        {{value}}

​                    </ul>

​                </li>

​            </ul>

​        </div>

​    </template>

​    <template id="index"> 

        <div>

​            <router-link to="/search">

​                <input type="text" class="search">

​            </router-link>

​            <router-link to="/shan">山东</router-link>

​            <router-link to="/henan">河南</router-link>

​            <router-view></router-view>

​        </div>

​    </template>

​    <template id="shan" >

        <div class="shan">

​            山东

​        </div>

​    </template>

​    <template id="henan">

        <div class="henan">

​            河南

​        </div>

​    </template>

</body>

<script>

​    //两个组件

​    let shandong={

​        template:"#shan",

​        data(){

​            return {

​                bol:false

​            }

​        }

​    }

​    let henan={

​        template:"#henan"

​    }

​    let index = {

​        template:"#index",

​    }

​    let search = {

​        template:"#search",

​        data(){

​            return {

​                search:"",

​                cityArr:[{

​                    sheng:"山东",

​                    city:["济南","青岛","潍坊"]

​                },{

​                    sheng:"河南",

​                    city:["安阳","郑州"]

​                }]

​            }   

​        },

​        computed:{

​            searchData(){

​                if(this.search){

​                    let resultArr = this.cityArr.filter(item=>{

​                        console.log(this.search)

​                        console.log(item.sheng.indexOf(this.search))

​                        if(item.sheng.indexOf(this.search)>=0){

​                            return true;

​                        }else{

​                            return false;

​                        }

​                    })

​                    console.log(resultArr)

​                    return resultArr

​                }else{

​                    return [];

​                }

​                

​            }

​        }

​    }

​    let routes=[

​        {

​            path:"/",

​            component:index,

​            children:[{path:'/shan',component:shandong},

​                {path:'/henan',component:henan}]

​        },

​        {

​            path:"/search",

​            component:search

​        }

​    ]

​    var router=new VueRouter({

​        routes    

​    })

​    var vm=new Vue({

​        el:"#app",

​        router

​    })

</script>

</html>
```

## vue05

### path路由传值

```
// 传值，要传的是变量

​    //定义一个组件

​    let user={

​        template:"#user"

​    }

​    //定义一个路由

​    let routes=[

​        // 在设置路由的时候  要传的值加在／:的后面

​        {path:"/user/:userName",component:user}

​    ]

​    //new路由

​    let router=new VueRouter({

​        routes

​    })

​    //挂载实例

​    let vm=new Vue({

​        el:"#app",

​        data:{

​            obj:{

​                userName:"shao"

​            }

​            

​        },

​        router,

​        methods:{

​            fn(){

​                // 跳转页面的存储对象==》locastorage

​                console.log(this.$route);

​            }

​        }

​    })
<div id="app">
<!-- path路由跳转的时候 命名路由    path+路由名称+传的数据 aa-->
<router-link :to="{path:'/user/aa'}">aa</router-link>
<router-link :to="{path:'/user/bb'}">bb</router-link>
<!-- 字符串拼接 -->
<router-link :to="{path:'/user/'+userName}">cc</router-link>
<!-- 按钮 -->
<button @click="fn">点击进入控制台</button>
<!-- 渲染 -->
<router-view></router-view>
</div>
<!-- 组件 -->
<template id="user">
<div class="user">
<!-- 我是user界面 -->
<!-- aa -->
<!-- 接收到数据，显示在组件中  localstorage.set    localstorage.get -->
{{$route.params.userName}}
<!-- {{}} -->
</div>
</template>
```

### query传值

```
let user={

​        template:"#user"

​    }

​    //根据需求画路由，决定多少组件，组件化开发

​    let routes=[

​        {path:"/user/:userName",component:user}

​    ]

​    let router=new VueRouter({

​        routes

​    })

​    let vm=new Vue({

​        el:"#app",

​        data:{

​        },

​        router,

​        methods:{

​            fn(){

​                console.log(this.$route)

​            }

​        }

​    })

​    // query传值会将你传的值显示在地址栏儿上，类似与我们的ajax中的get传输方式
<div id="app">
<router-link :to="{path:'/user/aa',query:{name:'shao',age:28}}">aa</router-link>
<router-link :to="{path:'/user/bb',query:{name:'che',age:18}}">bb</router-link>
<router-link :to="{path:'/user/cc',query:{name:'hui',age:16}}">cc</router-link>
<button @click="fn">点我进入控制台</button>
<router-view></router-view>
</div>
<!-- 组件 -->
<template id="user">
<div class="user">
{{$route.params}}
<!-- query可以和path连着用，接下来要讲的params传值方式不可以配合path -->
{{$route.query}}
</div>
</template>
```

### params传值

```
//根据路由画模版

​    let user={

​        template:"#user"

​    }

​    // 定义路由

​    let routes=[

​        {path:"/user",name:"user",component:user}

​    ]

​    //new 路由实例

​    let router=new VueRouter({

​        routes

​    })  

​    //挂在实栗

​    let vm=new Vue({

​        el:"#app",

​        data:{

​            userName:"shao",

​            age:28

​        },

​        router,

​        methods:{

​            fn(){

​                console.log(this.$route)

​            }

​        }

​    })

​        // params传值注意的点

​        // +命名路由name配合使用

​        // 不能和path配合使用(容易出现问题)

​        // 刷新时数据被清空

​        //meta用来拦截路由的 
 <div id="app">
 <!-- params传值不能配合我们的path  它是和name命名路由一起用 -->
 <router-link :to="{name:'user',params:{name:userName,age:age}}">用户</router-link>
 <button @click="fn">点我进入控制台</button>
 <router-view></router-view>
 </div>
 <!-- 模版 -->
 <template id="user">
 <div class="user">
 {{$route.params}}
 </div>
 </template>
```

###  动态路由

```
//动态路由

​    //定义四个组件

​    // let aa={tempplate:"#aa"}

​    // let bb={tempplate:"#bb"}

​    // let cc={tempplate:"#cc"}

​    // let dd={tempplate:"#dd"}

​    let user={template:"#user"}

​    //定义路由

​    // let routes=[

​    //     {path:"/aa",component:aa},

​    //     {path:"/bb",component:bb},

​    //     {path:"/cc",component:cc},

​    //     {path:"/dd",component:dd}

​    // ]

​    //path:"/user/:user"

​    let routes=[{

​        path:"/user/:userName/age/:age",

​        component:user

​    }]

​    let router=new VueRouter({

​        routes

​    })

​    let vm=new Vue({

​        el:"#app",

​        data:{

​            

​        },

​        router

​    })
<div id="app">
<router-link :to="{path:'/user/aa/age/20'}">aa</router-link>
<!-- <router-link :to="{path:'/user/bb'}">bb</router-link>
<router-link :to="{path:'/user/cc'}">cc</router-link>
<router-link :to="{path:'/user/dd'}">dd</router-link> -->
<router-view></router-view>
</div>
<!-- 四个组件 -->
<!-- 一个组件 -->
<template id="user">
<div class="user">
我是{{$route.params.userName}}页面
我{{$route.params.age}}岁了
</div>
</template>
```

### 点击进入详情页面案例

```
//根据路由写组件

​    let index={

​        template:"#index",

​        // 数据

​        data(){

​            return {

​                goods:[

​                    {

​                        // 给唯一的标识

​                        id:1,

​                        name:"黄瓜",

​                        price:24

​                    },

​                    {

​                        id:2,

​                        name:"西瓜",

​                        price:25

​                    },

​                    {

​                        id:3,

​                        name:"冬瓜",

​                        price:26

​                    }

​                ]

​            }

​        }

​    }

​    //详情组件

​    let details={

​        template:"#details"

​    }

​    //先去画路由

​    let routes=[

​        //主页路由

​        {

​            path:"/",//根目录下

​            component:index

​        },

​        //详情路由

​        {

​            path:"/details/:name/price/:price",

​            component:details

​        }

​    ]

​    //创建我们的router实例

​    let router=new VueRouter({

​        routes

​    })

​    //挂在我们的vue实例上

​    let vm=new Vue({

​        el:"#app",

​        data:{},

​        router

​    })
<!-- view层中写路由 -->
<div id="app">
<!-- <router-link to="/"></router-link> -->
<router-view></router-view>
</div>
<!-- 首页的内容，在组件中 -->
<template id="index">
<div class="index">
<!-- 将数据渲染出来 -->
<p v-for="(items,index) in goods"> 
<!--路由文件的跳转  -->
<router-link :to="{path:'/details/'+items.name+'/price/'+items.price}">
{{items}}
</router-link> 
<router-view></router-view>
</p>
</div>
</template>
<!-- detail组件 -->
<template id="details">
<div class="details">
{{$route.params}}
</div>
</template>
```

### 侦听器

```
 let vm=new Vue({

​        el:"#app",

​        data:{

​            xing:"",

​            ming:"",

​            fullName:""

​        },

​        //概念

​        // 检测数据,一般为普通的字符串，number ，bol

​        //假如想要监听对象和数组 

​        //双层对象

​        // {

​        //     obj:{

​        //         obj:{

​        //         }

​        //     }

​        // }

​        //深度监听  深度响应

​        //添加侦听

​        watch:{

​            //当你的姓改变的时候，整体的姓名也要改变

​            xing(){

​                this.fullName=this.xing+this.ming

​            }

​        }

​    })
<div id="app">
姓: <input type="text" v-model="xing"><br/>
名: <input type="text" v-model="ming"><br/>

{{fullName}}
</div>
```

### 深度响应

```
//创建一个实例子

​    let vm=new Vue({

​        el:"#app",

​        data:{

​            obj:{

​                name:"shao"

​            }

​        },

​        //设置监听

​        watch:{

​            obj:{

​                // 设置一个监听方法

​                // v1 指代 更新之前的数据 <==> v2指代的更新之后的数据 两者指向同一数据对象 

​                handler(v1,v2){

​                    console.log("哈哈哈")

​                    console.log(v1,v2)

​                },

​                // 深度监听 deep属性

​                deep:true

​            }

​        }

​    })
<div id="app">
<input type="text" v-model="obj.name">
</div>
```



# 修饰符

## native方法:

将原生事件绑定到组件上

```html
<div>
    <kky @click.native = "fn"></kky>
</div>
```

```js
var kky = {
	template:`
	<div class="kky">点我试试</div>
	`
}
```

此时不用在组件内部写点击事件的方法,直接可以用click方法

## promise函数

新的解决异步编程 方案

​        异步传统解决方案是回调函数

Promise有三个状态 有三种状:

​	pending(进行中),

​	fulfilled(已成功)

​	和rejected(已失败)

对应函数resolve  成功时的回调,rejected失败时的回调

格式:

```js
var p = new Promise((resolve, reject)=>{
    if(条件){
        resolve (成功时回调)
    }else{
        reject(失败时回调)
	}
})
```

then 

​    至少需要一个函数,成功时调用的函数对应resolve,

​    若有第二个函数对应reject函数,通常建议使用catch处理recject

​    catch 对应reject函数

​    finally 无论resove,还是reject,执行之后最后执行

​    

​    有三个异步函数分别为myajax1 ,myajax2,myajax3

​    all 三个都成功时,返回数据[数据1,数据2,数据3], 看谁最后成功,谁跑的慢(都需要成功)

​    

​    race 只要有一个成功,就返回数据,看谁最先成功 看谁跑的快(只要第一个成功,就不再执行后面的),第一个失败了,也不再执行

​    

```js
//三次调用异步,但无法控制异步顺序
			// myAjax(url1).then(res1=>{
			// 	console.log("res1:",res1);
			// }).catch(err1=>{
			// 	console.log("err1:",err1);
			// })
			// myAjax(url2).then(res2=>{
			// 	console.log("res2:",res2);
			// }).catch(err1=>{
			// 	console.log("err2:",err2);
			// })
			// myAjax(url3).then(res3=>{
			// 	console.log("res3:",res3);
			// }).catch(err3=>{
			// 	console.log("err3:",err3);
			// })
			
			//按照需要的顺序,调用三次异步
			myAjax(url2).then(res2=>{
				console.log("res2:",res2);
				return myAjax(url3);
			}).then(res3=>{
				console.log("res3:",res3);
				return myAjax(url1);
			}).then(res1=>{
				console.log("res1:",res1);
			}).catch(err=>{
				console.log("err:",err);
			})
			
```



## async   await函数

async await

​    使得异步操作变得更加方便 必须和promise连用

​    

​    格式

​    async 规定这是个async 函数 fn

​    await 必须放在一个通过promise写的异步函数

​    最后调用fn

   

​    fn().then().catch().finally();

​    

## 重定向

​      四种:

/*

​      redirect:"地址"(String)

​      redirect:{ptah:"/地址"},对象

​      redirect:{name:"路由名称"},对象

​      redirect:to=>{

​        //逻辑操作

​        return 新地址(字符串)

​      }

​       */

## 路由守卫

```js

router.beforeEach((to, from, next)=> {
  //console.log("00000000000000000to:",to,"form:",from,"是否需要登录:",to.meta.isLogin);
  //to.meta.isLogin//只能判断单个路由是否需要登录
  //to.matched.some(route=>route.meta.isLogin) //判断当前路由以及父路由是否需要登录
  document.title=to.meta.title;//地址栏的标题
  if(to.matched.some(route=>route.meta.isLogin)){
    if(localStorage.token){//是否登录
      next();
    }else{
      Message({
        message:"请先登录,即将前往登录页面",
        center:true,
        duration:1500
      });
      setTimeout(()=>{
        next({
          path:"/login",
          query:{redirect:to.fullPath}//登录后将回到登录之前的路由
        })
      })
    }
  }else{
    next();
  }
})
```

