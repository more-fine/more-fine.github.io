---
title: vacation知识点总结
date: 2020-09-18 00:15:04
tags: 
categories: 知识点总结
---
# 第二次

### 七、flex布局

回答思路：

1、flex的父元素的六个属性：

​		①display   =>    flex;

​		②flex-direction  =>  决定主轴的方向，它包括row, row-reverse, column, column四个值，默认是row，从左向右排列元素

​		③flex-wrap  =>  它有三个值：wrap（空间不足时换行），nowrap（空间不足时也换行，会挤在一起,这也是默认值），wrap-reverse(换行，但是第一行在下面)

​		④flex-flow =>flex-direction 和flex-wrap的简写，属性是【flex-direction，flex-wrap】的属性值

​		⑤justify-content =>  flex-start  、 flex-end  、 center 、 space-between  、 space-around

​		⑥align-items  => flex-start   、 flex-end 、 center 、baseline  、 stretch 

项目元素（子元素）的六个属性：

- `order`
- `flex-grow`
- `flex-shrink`
- `flex-basis`
- `flex`
- `align-self`

2、工作中写flex布局遇到的问题：

# 第三次

# 第四次

## 19、`cookie`和 `sessionStorage`和` localStorage`的区别：

### 回答问题的思路：

#### 共同点：

都保存在浏览器端，且是同源的（顺便解释一下同源：域名、协议、端口号相同）

#### 不同点：

##### 存储大小不同：

cookie大小一般4k，sessionStorage一般5M，localStorage一般为5M

##### 有效期不同：

cookie:设置有效期之前有效，当超过有效期之后会失效。  localstorage:永久有效，除非你进行手动删除 

sessionStorage:当前会话有效，关闭浏览器会失效

##### sessionStorage和localStorage的作用域的区别详情：

不同浏览器无法共享localStorage和sessionStorage中的信息。

相同浏览器的不同页面间可以共享相同的localStorage（页面属于相同域名和端口），但是不同页面或标签页面无法共享sessionStorage的信息。

### 带入项目：

window.location.href跳转页面的时候会丢失cookie,不是其他的什么原因，而是需要设置cookie作用域的路径path

localhost使用出现的问题：

localStorage和sessionStorage兼容到ie8，判断浏览器是否支持：

```html
if（window.localStorage）{
	alert("浏览器支持localStorage")
}
```

存：localStorage.setItem("arr",JSON.stringify(data.body.data)),需要转换成字符串，不能直接存对象，切记

取：JSON.parse(localStorage.arr)

## 20、0.1+0.2!=0.3怎么处理

机器精度---误差范围，一般为2-52

考察js中的数值的理解程度

解决方案：

快速说出因为js的浮点数运算不精确的问题，说出两种解决浮点数问题的方案

涉及、扩展：

方法1：通过toFixed（num）方法来保留小数，计算结果不精确

方法2：把计算的数值升级，乘以10的n次方，转换成计算机能够识别的精确的整数，再降幂，推荐使用。

## 21、数组的常用方法：

①　push( ) 添加：向数组末位添加一个会多个值。unshift（）向数组开头添加一个或多个值。
②　pop( ) 删除：删除数组中最后一个值。shift（）删除数组第一个值。
③　indexOf（）查找：查找数组中是否有某一个值，存在返回其下标，不存在返回-1.
 lastIndexOf()查找最后一个重复，返回其下标值。
④　join()转换：把数组转换为字符串，括号内填写连接符，默认逗号隔开。
⑤　sort()排序：从大到小或从小到大，字符串从a-z；
⑥　concat()拼接：将两个数组拼接在一起形成一个新的数组。
⑦　reverse()反转：将数组值反序排列。
⑧　forEach()遍历：遍历数组，每个元素都执行回调函数。
⑨　splice（x,n，z）添加/修改/删除：从下标x（包含x），{从第几个开始删除，删除几个，替换}  删除n个值（值为为0实现添				加），添加z值。删除几个添加几个对应的值实现修改。
⑩　filter（）查找到符合条件的值，返回新的数组。
⑪　find（）返回第一个符合条件的数组值。
⑫　findIndex（）返回第一个符合条件数组值的下标。
⑬　include（）查找：能查到返回true，不存在返回false。（Ie不支持）
⑭　every() 验证数组中的每个值是否都符合条件，是为true，一个不符合，返回false。
⑮　fill() 修改：使用一个值来替换任意值，包括开头，不包括结束。
⑯　Array.isArray() 检测是否是数组。
⑰　map（）处理数组的每一个值，返回处理后的结果。
⑱　some() 检测数组中是否有一个值符合条件，是返回true，全部都不符合，返回false。

## 22、new对象的四个过程 

1、创建一个空对象

```
var obj = new Object();
```

2、让person中的this指向obj，并执行person的函数体

```
var result = person.call(obj);
```

3、设置原型链，将obj的`__proto__`成员指向了person函数对象的prototype成员对象

```
obj.__proto__ = person.prototype;
```

4、判断person的返回值类型，如果是值类型，返回obj。如果是引用类型，就是返回这个引用类型的对象。

```
if(typeof(result)=="object"){
	person = result
}else{
	person = obj
}
```

### 涉及、扩展

1，构造函数的写法

2，构造函数中的this指向，指向当前实例化的对象

3，prototype  `__proto__` 是什么？

prototype：每个函数都有的一个prototype属性，该属性是一个指针，指向一个对象。而这个对象的用途式包括由特定类型的所有实例共享的属性和方法。使用这个对象的好处就是可以让所有的实录i对象共享它所拥有的属性和方法。

`__proto__` :每个实例化对象都有一个`__proto__`属性，用于指向构造函数的原型对象，`__proto__` 属性实在调用构造函数创建的实例对象时产生的。 

### 遇到的问题：

构造函数与一个普通函数并无不同，如果你故意不使用new，或者忘记用new，都会出现奇怪的现象。

构造函数自身有返回值

简单总结：

显式的返回值是以下的值：undefined、null 、 boolean 、number等基础类型，并不会代替new式调用的默认行为

但是显式返回的值：{}，[],RegExp,Date,Function,均会代替new调用的默认返回值this

## 23、JS的继承

①原型链继承
重点：让新实例的原型等于父类的实例。
　　　　特点：1、实例可继承的属性有：实例的构造函数的属性，父类构造函数属性，父类原型的属性。（新实例不会继承父类实例的属性！）
　　　　缺点：1、新实例无法向父类构造函数传参。
　　　　　　　2、继承单一。
　　　　　　　3、所有新实例都会共享父类实例的属性。（原型上的属性是共享的，一个实例修改了原型属性，另一个实例的原型属性也会被修改！）
②二、借用构造函数继承
　　　　重点：用.call()和.apply()将父类构造函数引入子类函数（在子类函数中做了父类函数的自执行（复制））
　　　　特点：1、只继承了父类构造函数的属性，没有继承父类原型的属性。
　　　　　　　2、解决了原型链继承缺点1、2、3。
　　　　　　　3、可以继承多个构造函数属性（call多个）。
　　　　　　　4、在子实例中可向父实例传参。
　　　　缺点：1、只能继承父类构造函数的属性。
　　　　　　　2、无法实现构造函数的复用。（每次用每次都要重新调用）
　　　　　　　3、每个新实例都有父类构造函数的副本，臃肿。
③　三、组合继承（组合原型链继承和借用构造函数继承）（常用）
　　　　重点：结合了两种模式的优点，传参和复用
　　　　特点：1、可以继承父类原型上的属性，可以传参，可复用。
　　　　　　　2、每个新实例引入的构造函数属性是私有的。
　　　　缺点：调用了两次父类构造函数（耗内存），子类的构造函数会代替原型上的那个父类构造函数。
④　四、原型式继承 
　　　　重点：用一个函数包装一个对象，然后返回这个函数的调用，这个函数就变成了个可以随意增添属性的实例或对象。object.create()就是这个原理。
　　　　特点：类似于复制一个对象，用函数来包装。
　　　　缺点：1、所有实例都会继承原型上的属性。
　　　　　　　2、无法实现复用。（新实例属性都是后面添加的）
⑤寄生式继承
　　　　重点：就是给原型式继承外面套了个壳子。
　　　　优点：没有创建自定义类型，因为只是套了个壳子返回对象（这个），这个函数顺理成章就成了创建的新对象。
　　　　缺点：没用到原型，无法复用。
⑥寄生组合式继承（常用）
　　　　寄生：在函数内返回对象然后调用
　　　　组合：1、函数的原型等于另一个实例。2、在函数中用apply或者call引入另一个构造函数，可传参　
　　　　 
　　　　重点：修复了组合继承的问题

## 24、get和post的区别

1. get是不安全的，因为在传输过程，数据被放在请求的URL中post的所有操作对用户来说都是不可见的。
2. get传送的数据量较小，这主要是因为受URL长度限制；post传送的数据量较大，一般被默认为不受限制。
3. get限制form表单的数据集的值必须为ASCII字符；而post支持整个ISO10646字符集。
4. get执行效率却比post方法好。get是form提交的默认方法。 收起  

# 第五次

# 第六次

## 31、性能优化

### 1、分析题目：回答

网站的性能优化，如何做优化，用户体验度

### 2、面试官员想要的答案：

对网页性能的优化

通过哪些方式进行优化

如何减轻服务器的压力的

### 3.回答该问题思路

```
  一、 内容层面
      1、DNS解析优化(DNS缓存、减少DNS查找、keep-alive. 适当的主机域名)
      2、避免惠害向，(还是需要的)
      3、切分到多个域名
      4、杜绝404
  二、网络传输阶段
  1.减少传输过程中实体的大小
      1)缓存
      2) cookie优化
      3)文件压缩(Accept- Encoding: g-zip)
  2、减少请求的次数
      1、文件适当的合并
      2、雪碧图
      3、1异步加载(并发,requirejs)
      4、预加载、延后加载、按需加载上
  三、渲染阶段
  1、js放底部，  css放顶部2、减少重绘和回流
  3、合理使用Viewport等meta头部4、减少dom节点5、BigPipe
  四、脚本执行阶段
    1、缓存节点，尽量减少节点的查找
    2、减少节点的操作(innerHTML)
    3、避免无谓的循环，break、 continue、 return的适 当使用	
    4、事件委托
```

### 4、涉及、扩展的知识点

回答：性能优化、项目的工程化、前期项目的构建

### 5、带入项目，使用场景

![img](/img/2021/22.png)

### 6、使用过程中可能出现的问题？以及如何解决？

回答：复用组件时，有可能出现互相影响的情况，

解决：降低耦合度（高内聚，低耦合思想），高度解耦

## 32、对MVC和MVVM的理解

1、编程中共有28中设计模式，其中MVC最为经典

例：工厂模式、单例模式、观察者模式等

2、M：Model（数据层）V：View（视图层-页面）C：Controller（控制器）-就是一个作用域对象。

3、MVVM：M：代表Model、V：代表View、VM：代表ViewModel

4、VM就是讲视图（view）和数据层（model）连接起来，实现了数据双向绑定

5、MVC体现产品为Angular框架，MVVM体现产品为Vue框架

### 扩展知识点：

1、 模块化思想

例：AMD、CMD、CommonJS、ES6 Module等

2、设计模式

3、模块化编程，集中管理

4、数据双向绑定

例：Vue数据双向绑定原理：数据劫持

Angular数据双向绑定原理：脏检查模式

5、带入项目，使用场景

![](/img/2021/23.png)

## 33、Vue2双向数据绑定原理

Vue采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty劫持data属性的setter、getter，在数据变动时发布消息给订阅者，触发相应的监听回调。

1、          Object.defineProperty()

Object.defineProperty()方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回这个对象。

   Get：

​      一个给属性提供getter的方法，如果没有getter则为undefined。当访问该属性时，该方法会被执行，方法执行时没有参数传入，但是会传入this对象（由于继承关系，这里的this并不一定是定义该属性的对象）。默认为undefined。

Set：

  一个给属性提供setter的方法，如果没有setter则为undefined。当属性值修改时，触发执行该方法。该方法将接受唯一参数，即该属性新的参数值。默认为undefined。

### 扩展知识点：

1、vue双向绑定

![](/img/2021/clip_image002.jpg)

具体步骤：

![](/img/2021/clip_image004.jpg)

2、vue3核心原理Proxy（es6）

![](/img/2021/clip_image006.jpg)

3、          使用过程中可能会出现的问题，以及如何解决？

Object.defineProperty()只能对属性进行数据劫持，不能对整个对象进行劫持，同理无法对数组进行劫持

解决方案：vue提供了Vue.set(object,propertyName,value)/vm.$set(object,propertyName,value),来实现为对象添加响应属性。

## 34、v-model双向绑定原理

v-model指令可以在表单`<input>`、`<textarea>`及`<select>`元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。但v-model本质上不是语法糖。

完整写法：`<input v-bind:value="" v-on:input=""/>`

v-model会忽略所有表单元素的value、checked、selected特性的初始值，而总是将Vue实例的数据作为数据来源。你应该通过JavaScript在组件的data选项中声明初始值。

v-model在内部为不同的输入元素使用不同的属性并抛出不同的事件：

text和textarea元素使用value属性和input事件

checkbox和radio使用checked属性和change事件

select字段将value作为prop并将change作为事件

### 扩展知识点：

1、          修饰符

v-model.lazy只有在input输入框发生一个blur时才触发

v-model.trim将用户输入的前后的空格去掉

v-model.number将用户输入的字符串转换成number

带入项目，使用场景

![](/img/2021/clip_image008.jpg)

![](/img/2021/clip_image010.jpg)

![](/img/2021/clip_image012.jpg)

 

## 35、vue的生命周期

vue实例从创建到销毁的过程，就是生命周期。也就是从开始创建、初始化数据、编译模板、挂载Dom->渲染、更新->渲染、销毁等一系列过程，称之为vue生命周期。

### 扩展知识点：

![](/img/2021/clip_image014.jpg)

### 带入项目，使用场景

![](/img/2021/clip_image016.jpg)

### 使用过程中可能会出现的问题，以及如何解决？

![](/img/2021/clip_image018.jpg)

 

## 36、组件data为什么返回函数

如果是对象，对象是对同一地址的引用，而不是独立存在的。会造成数据污染。如果写成函数的话，那么他们有一个作用域的概念在里面，相互隔阂，不受影响。每一次都是新的数据。

1、Data数据如何绑定到view：

v-model：主要提供了两个功能，view层输入值影响data的属性值，data属性值发生改变会更新view层的数值变化。

2、为什么使用组件

1）组件比应用程序小，比类大，如果接口相同还可以替换原来组件。可实现无缝升级。

2）组件功能是独立的，可以重复使用。

3）减少代码量，使得代码更容易维护。

4）是可扩展的html元素，也是Vue实例。

5）可以接受相同的选项对象（除了一些根级特有的选项）并提供相同的生命周期钩子。

3、如何创建组件

全局组件  局部组件

脚手架：.vue独立组件

4、          组件通信

![](/img/2021/clip_image020.jpg)

 

# 第七次

