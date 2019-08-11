

### 什么是Vue.js

- Vue.js 是目前最火的一个前端框架，React是最流行的一个前端框架（React除了开发网站，还可以开发手机App， Vue语法也是可以用于进行手机App开发的，需要借助于Weex）

- Vue.js 是前端的**主流框架之一**，和Angular.js、React.js 一起，并成为前端三大主流框架！

- Vue.js 是一套构建用户界面的框架，**只关注视图层**，它不仅易于上手，还便于与第三方库或既有项目整合。（Vue有配套的第三方类库，可以整合起来做大型项目的开发）

- 前端的主要工作？主要负责MVC中的V这一层；主要工作就是和界面打交道，来制作前端页面效果；

  #### 双向数据绑定的概念

  定义：通过框架提供的指令，我们前端程序员只需要关心数据的业务逻辑，不再关心DOM是如何渲染的了

  #### vue的核心概念

  在Vue中，一个核心的概念，就是让用户不再操作DOM元素，解放了用户的双手，让程序员可以更多的时间去关注业务逻辑；

  

### 框架和库的区别

框架：是一套完整的解决方案；对项目的侵入性较大，项目如果需要更换框架，则需要重新架构整个项目。

比如：node 中的 express；

库（插件）：提供某一个小功能，对项目的侵入性较小，如果某个库无法完成某些需求，可以很容易切换到其它库实现需求。

比如：

- 1. 从Jquery 切换到 Zepto
- 2. 从 EJS 切换到 art-template







### MVVM前端视图层分层开发思想

主要把每个页面，分成了M   V  和 VM ,其中，VM是MVVM思想核心，因为VM是M和V之间的调度者

M：这里的M保存的是每个页面中单独的数据

VM：它是一个调度者，分割了M和V。每当V层想要获取后保存数据的时候，都要有VM做中间的处理

V：就是每个页面中的HTML结构

前端页面中使用MVVM的思想，主要是为了让我们开发更加方便，因为MVVM提供了数据的双向绑定

注意：数据的双向绑定是有VM提供的

![1565492754538](C:\Users\87625\AppData\Roaming\Typora\typora-user-images\1565492754538.png)





### Vue指令

#### v-cloak/v-text/v-html

v-cloak:

cloak:隐藏  

使用v-cloak能够解决 插值表达式闪烁的问题

```html
<p v-cloak>+++{{ msg }}---good</p>
```

v-text:

v-text会覆盖元素中原本的内容

```html
<p v-text= msg1> ---good--- </p>
```

v-html:

v-html 会把里面的标签解析成html的标签也，会覆盖元素中原本的内容

```html
<div v-html= msg2>{{ msg2 }}</div>
```

区别：

插值表达式只会替换自己的这个占位符，不会把整个元素的内容清空



#### v-bind

v-bind 是 Vue中，提供的用于绑定属性的指令,可以合法的写js表达式

```html
<input type="button" value="按钮" v-bind:title="mytitle">
```

注意：v-bind 指令可以被简写为  :  要绑定的属性 也就是一个冒号

```html
<input type="button" value="哈哈" :title='mytitle1'>
```



#### v-on

Vue 中提供了 v-on：事件绑定机制

Vue 中提供了 v-on：指令可以被简写为 @

methods: 这个 methods属性中定义了当前的Vue实例所有可用的方法

```html
<input type="button" value="嘿嘿" v-on:click='show'>
```

```js
 methods:{ //这个 methods属性中定义了当前的Vue实例所有可用的方法
            show: function() {
                alert('你好啊')
            }
```



#### v-model 和 双向数据绑定

使用 v-model 指令，可以实现表单元素和 Model 中数据的双向绑定

注意：v-model 只能运用在 表单元素中 例如： input(radio,text,address,email...)  select  checkbox   textarea



#### v-for 和 key 属性

1.迭代数组

```html
<!-- 此时如果想拿到索引，用一个小括号包起来， item代表我们循环的每一项，i 代表索引，和我们以前的  数组.forEach((item,i) =>{})一样，里面传递一个回调函数的时候，传递一个参数就是代表每个元素，两个就代表一个元素，一个索引-->
<p v-for="(item,i) in list">索引值是:{{ i }} ---- 每一项是:{{ item }}</p>
```

2.迭代对象

```html
<p v-for="(item,i) in list">id:{{item.id}}---名字:{{item.name}}---索引:{{i}}</p>
```

```js
var vm = new Vue({
        el:'#app',
        data:{
            list:[
                {id:1, name:'光头强'},
                {id:2, name:'熊大'},
                {id:3, name:'熊二'},
                {id:4, name:'熊三'},
            ]
        },
        methods:{}
    })
```

3.迭代对象中的属性

```html
<!-- 注意：在遍历对象身上的键值对的时候，除了 有 val key, 在第三个位置还有一个索引 -->
        <p v-for="(val,key,i) in user">值:{{ val }}----键:{{ key }}---索引:{{ i }}</p>
```

```js
 var vm = new Vue({
        el:'#app',
        data:{
            user: {
                id:1,
                name:'光头强',
                gender:'男'
            }
        },
        methods:{}
    })
```



4迭代数字

```html
<!-- in 后面我们放过  普通数据  对象数组 对象 还可以放数字 -->
         <!-- 注意：如果使用 v-for 迭代数字的话， 签名的 count 值从 1 开始 -->
        <p v-for="count in 10">这是第{{ count }}次循环</p>
```

注意点：2.2.0+ 的版本里，**当在组件中使用** v-for 时，key 现在是必须的。



当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用 “**就地复用**” 策略。如果数据项的顺序被改变，Vue将**不是移动 DOM 元素来匹配数据项的顺序**， 而是**简单复用此处每个元素**，并且确保它在特定索引下显示已被渲染过的每个元素。



为了给 Vue 一个提示，**以便它能跟踪每个节点的身份，从而重用和重新排序现有元素**，你需要为每项提供一个唯一 key 属性。



#### 事件修饰符：

.stop       阻止冒泡

.prevent    阻止默认事件

.capture    添加事件侦听器时使用事件捕获模式

.self       只当事件在该元素本身（比如不是子元素）触发时触发回调

.once       事件只触发一次





### 在Vue中使用样式

#### 使用class样式

1.数组

```html
<h1 :class="['pink','italic']">我是光头强，我的名字非常大</h1>
```

2.数组中使用三元表达式

```html
<h1 :class="['pink','italic', flag?'active' : '']">我是光头强，我的名字非常大</h1>
```

3.数组中嵌套对象

```html
<h1 :class="['pink','italic', {'active':flag}]">我是光头强，我的名字非常大</h1>
```

4.直接使用对象

```html
<h1 :class="{ pink:true,thin:true,italic:true,active:false  }">我是光头强，我的名字非常大</h1>
```



#### 使用内联样式

1.直接在元素上通过 `:style` 的形式，书写样式对象

```
<h1 :style="{ color:'pink','font-weight':200 }">我是熊大，俺是熊二，我们两只熊的名字很大</h1>
```

2.将样式对象，定义到 `data` 中，并直接引用到 `:style` 中

在元素中，通过属性绑定的形式，将样式对象应用到元素中：

```html
<h1 :style="styleObj1">我是熊大，俺是熊二，我们两只熊的名字很大</h1>
```

```js
在data上定义样式：
data:{
       styleObj1:{ color:'pink','font-weight':200 },
 }
```

3.在 `:style` 中通过数组，引用多个 `data` 上的样式对象

在元素中，通过属性绑定的形式，将样式对象应用到元素中：

```html
<h1 :style="[styleObj1,styleObj2]">我是熊大，俺是熊二，我们两只熊的名字很大</h1>
```

在data上定义样式：

```js
data:{
      styleObj1:{ color:'pink','font-weight':200 },
      styleObj2:{ 'font-style':'italic' }
}
```











































