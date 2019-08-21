

### webpack

> 最佳体验
>
> 安装webpack的时候，安装本地和全局
>
> npm init -y
>
> npm i webpack webpack-cli -g
>
> npm i webpack webpack-cli -S



#### webpack体验

- 安装

  1.npm i webpack -S

  2.npm i webpack-cli -S

- 体验：使用Webpack进行打包转换

1.输入命令：webpack 需要进行打包构建的源文件

2.它会自动的将这个源文件打包构建放置到当前根目录下的dist文件夹中，并且生成一个名称为main.js的目标文件

3.这个main.js就是浏览器可以识别的文件，意味着在index.html文件中应该引入main.js文件

4.你指定了源文件进行打包构建，那么这还会同时打包构建这个源文件中引用的其它所有资源文件

**5.命令：webpack 源文件****



- 细节：mode设置

  1.默认为product:产品阶段，会将生成的目标js文件进行压缩

  2.设置mode:--mode=development：生成一个未压缩的源js文件





#### webpack.config.js

这 个文件是webpack的核心配置文件，我们在这个文件中可以进行如下配置：

1.入口

2.输出

3.loader加载器

4.plugin插件

- 如何创建webpack.config.js：在当前项目根目录 创建，同时注意名称死都不能改
- 如何配置入口
- 如何配置输出

```vue
var path = require('path')
// 进行webpack打包构建时的配置，它是一个配置对象
// 这个配置以后需要引入之后才能使用:当然,不用我们自己引入,而是webpack自动引入的
module.exports = {
    // 入口:你想打包构建的源文件路径--相对路径
    entry:'./src/app.js',
    // 输出:指定将源文件打包构建放置到?个文件夹?个文件名称
    output:{
        // 这个路径从3.*版本开始,就需要是一个绝对路径
        // 指定文件目录
        path:path.join(__dirname, 'dist'),
        // 指定文件名称
        filename:'bundle.js'
    }
}
```

享受配置成果：webpack:会将./src/app.js打包构建为bundle.js文件并存储在dist文件夹中，html页面就应该引入bundle.js文件



#### webpack-dev-server--重点

通过之前的配置，我们已经可以将指定的源文件打包构建为浏览器可以识别的文件，并正确的解析，但是我们发现一个问题：如果你修改了源代码，页面中并不能及时出现修改代码之后的结果：

原因是你的目标文件并没有更新>>你没有重新打包构建，目标文件中的功能没有变化

我们希望：修改源代码之后，自动的进行重新的打包构建，并是帮助我刷新浏览器



webpack-dev-server的作用：

安装：npm install webpack-dev-server --save-dev

配置

```vue
// 配置dev-server
devServer:{
    // 设置你的托管资源的存放目录,同时这个目录提供外部的访问,默认会生成一个main.js
    publicPath:'/dist'
}
```

享受成果

- 我们现在在页面中引入的资源不再是本地资源，而是服务器资源
- 意味着本地已经看不到打包构建好的目标文件了，因为它已经被托管到服务器上
- 在html页面中，我们可以以服务器资源的方式来请求目标文件

补充：webpack-dev-server --open:它可以自动的打开浏览器





#### webpack-css

让webpack能够支持css的解析处理

经典错误：因为没有这种类型的文件的对应的loader

![webpack-css错误配置提示](E:\前端笔记\images\webpack-css错误配置提示.png)

找到合适的loader:百度-webpack支持css（再去github取搜索配置）

下载安装：npm install css-loader style-loader --save-dev

配置

```vue
// 这个模块专门用于加载loader
module: {
    // 加载规则:什么类型的文件你要使用什么样的loader进行解析处理
    rules: [
        // 添加对css的支持
        {
            // 正则表达式:意味着后期以.css结尾的文件都会使用下面指定的loader来处理
            test: /\.css$/i,
            // css-loader:将css模块处理解析为浏览器可以识别的css代码
            // style-loader:将解析过后的Css代码添加到页面中
            // 加载loader是从右到左
            use: ['style-loader', 'css-loader'],
        },
    ],
}
```





#### webpack-less

less:预处理器，用来更方便简洁高效的创建样式

找到合适的less加载器

下载安装:npm install less less-loader -S

配置

```vue
// 添加对less的支持
{
    test: /\.less$/,
        use: [{
            loader: 'style-loader'
        }, {
            loader: 'css-loader'
        }, {
            loader: 'less-loader'
        }]
}
```





#### webpack-图片&字体

下载安装：npm install file-loader url-loader --save-dev

配置：

```js
 // 添加对图片和图标的支持
{
    // png|jpg|gif:常见的图片资源
    // eot|svg|ttf|woff:字体图标或web字体的字体源文件
    test: /\.(png|jpg|gif|eot|svg|ttf|woff)/,
        use: [{
            loader: 'url-loader',
            options: {
                // limit表示如果图片大于50000byte，就以路径形式展示，小于的话就用base64格式展示
                limit: 5000
            }
        }]
}
```





#### webpack-babel

可以将ES6转换为ES5，以便让浏览器能够支持我们所创建的代码

一些低版本的浏览器不支持





#### html-webpack-plugin

它可以自动的将打包构建好的js文件引入到html文件中

它甚至还可以自动的生成模板文件(*.html)

下载安装：npm i --save-dev html-webpack-plugin

配置

```vue
const HtmlWebpackPlugin = require('html-webpack-plugin')
--------------------------------------------------------
plugins: [
    new HtmlWebpackPlugin({
        // 指定模板源文件
        template:'index111.html',
        // 将模板文件打包构建为目标文件
        filename:'index.html',
        // 指定js文件的插入位置
        inject:'head'
    })
]
```

细节：这个插件与webpack-dev-server冲突。如果想要这个插件起作用就要注释到dev-server的配置



#### vue-cli

我们以后搭建一个基于Webpack的项目，需要先进行webpack配置，手动配置太繁琐

vue-cli就是一个帮助我们快速构建基于Webpack打包构建项目的工具，通过vue-cli可以快速的生成基于webpack的项目，意味着大量的配置不用你自己手动实现，因为这个工具已经为你生成好了

**使用过程**

下载安装：npm install -g @vue/cli

创建项目：vue create 项目名称

通过箭头切换选项，通过空格来确认选择，通过enter来确认到下一步

第一步：选择 CSS Pre-processors  记住：用空格先确定，前面会出现 * 号，就表示选中了，再按enter

![06-cli-1](E:\前端笔记\images\06-cli-1.png)

第二步：选择Less 预处理器

![07-cli-2](E:\前端笔记\images\07-cli-2.png)

第三步：选择eslint的配置

![08-cli-3](E:\前端笔记\images\08-cli-3.png)

第四步：选择默认，下一步

![09-cli-4](E:\前端笔记\images\09-cli-4.png)

第五步：选择第一个

![10-cli-5](E:\前端笔记\images\10-cli-5.png)

完成之后：可以看到结构

![23-cli项目结构](E:\前端笔记\images\23-cli项目结构.png)

**运行项目：npm run serve**



#### eslint

我们在项目中启用了eslint语法规范检测，往往为了代码能够满足这样的规范，我们可以这样处理

1.安装一个插件：eslint

2.添加用户设置：文件》首选项》设置>eslint > 打开setting.json文件

3.在vs code 里面安装，装一次就行了

配置代码：

```vue
{
  "editor.fontSize": 20,
  "liveServer.settings.donotShowInfoMsg": true,
  "javascript.updateImportsOnFileMove.enabled": "always",
  "files.autoSave": "off",
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "vue-html",
    {
      "language": "vue",
      "autoFix": true
    },
    "html",
    "vue"
  ],
  "eslint.autoFixOnSave": true,
  // 需要 npm install -g eslint-plugin-vue
  "eslint.options": {
    "extensions": [
      ".js",
      ".vue"
    ]
  },
  "explorer.confirmDragAndDrop": false,
}
```

4.重启vs-code





### 常见系统指令

指令在vue开发过程中非常常用，可以说没有指定，你使得vue开发寸步难行



#### 插值表达式

- 作用：展示在data中定义的数据，

- 表现形式：{{}}，只能写在标签之间，只能使用在双标签，它不能当成标签的属性来使用

- 使用方式{{变量}}：这个变量必须先定义，一般在data函数中定义

- 细节

  ```vue
  <div class="first">
      <p>{{msg}}</p>
      <!-- <p value='{{msg}}'></p> -->
      <!-- 1.拼接 字符串 -->
  	<p>{{msg + 'jack'}}</p>
      <!-- 2.实现简单的运算-表达式 -->
  	<p>{{age + 1}}</p>
      <!-- 3.使用api -->
  	<p>{{msg.toUpperCase().substr(1)}}</p>
      <p>{{gender?'男':'女'}}</p>
      <!-- 不能写js逻辑表达式 -->
      <!-- <p>{{if(gender){'男'}else{'女'}}}</p> -->
  </div>
  ```

  常见错误：

  ![11-插值表达式](E:\前端笔记\images\11-插值表达式.png)



#### v-text

它的使用方式类似于属性的使用：v-text='定义好的变量名称'

```vue
<div class="text">
        <p>这个文件描述v-text的使用</p>
        <p>v-text的作用和插值类似，都是为双标签添加展示内容,但是v-text是将指定的属性完全的替换标签之间的内容，而不管标签之间有什么</p>
        <hr>
        <p>{{msg + 'aaa'}}</p>
        <p v-text='msg'>{{name}}</p>
        <!-- 使用细节 -->
        <p v-text='msg+"你好"'></p>
        <p v-text='msg.toUpperCase()'></p>
        <p v-text='age + 1'></p>
        <p v-text='age>18?"成年":"未成年"'></p>
    </div>
```





#### v-html

作用：可以解析html结构

示例：

```vue
<div class="ht">
        <p>这个文件描述v-html</p>
        <p>v-html可以解析网页结构</p>
        <p>{{content}}</p>
        <p v-text='content'></p>
        <p v-html='content'></p>
    </div>
```

使用场景：以后可以接收从服务器返回的html结构代码并进行解析，在页面中展示



#### v-bind(重点)

它可以为属性动态绑定数据

当以后你的属性会是变化的，那么就应该使用v-bind

语法 ：< 标签  v-bind:属性=‘data中定义的变量’>

示例：

```vue
<div class="bind">
        <p>{{msg}}</p>
        <!-- <img src="/assets/logo.png" alt=""> -->
        <img v-bind:src="img" alt="">
        <!-- <a v-bind:href="'/del?id='+id">删除</a> -->
        <p>简写</p>
        <a :href="'/del?id='+id">删除</a>
        <p :[prop]='username'>动态绑定属性</p>
        <p>下面的示例为元素动态绑定样式</p>
        <p :class="{isRed:hasRed}">我是红色字</p>
        <!-- 实现左侧展开和合并 -->
        <button @click='isCollpse=!isCollpse'>单击展开和合并</button>
        <div :class='{left:true,collpse:isCollpse}'></div>
        <!-- <div :class="[classA, classB]"></div> -->
        <p :class="[isred,isunder]">没写字</p>
        <p :style='{fontSize:fontsize+"px"}'>难道大小是30？</p>
    </div>
```

简写方式：< 标签  :属性=‘data中定义的变量’>

有一个意识：任何属性都可以动态绑定



#### v-for(重点)

这个指令用于循环动态动态结构

在vue中只有一种：v-for

你想循环动态生成什么结构，就在这个结构中添加v-for

- 语法：<标签 v-for='(value,index) in 遍历源'>
- 遍历数组
- 遍历对象
- 细节说明

```vue
<template>
    <div class="for">
        <p>这个文件描述v-for的使用</p>
        <!-- 遍历数组 -->
        <p>对于数组，可以遍历出数组的value和index (值和索引)</p>
        <p>:key:可以唯一标识当前数据行，后期的操作可以提高效率，这个值必须是唯一值</p>
        <ul>
            <li v-for='(value,index) in arr' :key='index'>{{value + ":"+ index}}</li>
        </ul>
        <!-- 遍历对象 -->
        <p>对于对象，可以遍历出对象的value,key,index,(值，key,index)</p>
        <div v-for='(v,k,i) in obj' :key='k'>{{k+":"+v+":"+i}}</div>
        <!-- 遍历对象数组 -->
        <table border="1" width='600px'>
            <thead>
                <th>姓名</th>
                <th>年龄</th>
                <th>性别</th>
            </thead>
            <tbody>
                <tr v-for='(value,index) in userlist' :key='index'>
                    <td>{{value.name}}</td>
                    <td>{{value.age}}</td>
                    <td>{{value.gender}}</td>
                    <!-- <td v-for='(v,k) in value' :key='k'>{{v}}</td> -->
                </tr>
            </tbody>
        </table>
    </div>
</template>
<script>
export default {
  data () {
    return {
      arr: [1, 3, 5, 7, 9, 11],
      obj: {
        name: 'jack',
        age: 20,
        gender: '男'
      },
      userlist: [
        { name: 'jack',
          age: 21,
          gender: '男' },
        { name: 'rose',
          age: 18,
          gender: '女',
          subject: '大前端' },
        { name: 'tom',
          age: 19,
          gender: '男' }
      ]
    }
  }
}
</script>
<style lang="less" scoped>

</style>
```





#### v-model(重点)

> 实现数据的双向绑定：
>
> 1.修改数据，页面中的展示自动变化
>
> 2.修改页面中的数据，data中的数据也会自动的变化

- 语法： v-model='data中声明的成员'

- 使用限制：只有input  ,textarea, select可以使用

  ```vue
  <template>
      <div class="model">
          <p>这个文件讲解v-model双向数据绑定</p>
          <!-- v-model只支持：input select textarea -->
          <!-- <p v-model='username'>aa</p> -->
          <p>登陆</p>
          用户名：<input type="text" v-model='loginForm.username'> <br>
          密码：<input type="text" v-model="loginForm.userpass"> <br>
          <input type="button" value="登陆" @click='login'>
      </div>
  </template>
  <script>
  export default {
    data () {
      return {
      //   username: '',
      //   userpass: ''
        loginForm: {
          username: 'jack',
          userpass: '123456'
        }
      }
    },
    methods: {
      login () {
        console.log(this.loginForm)
      }
    }
  }
  </script>
  <style lang="less" scoped>
  
  </style>
  
  ```

  使用场景：表单元素的数据默认展示，收集表单数据



#### v-on(重点)

为元素绑定事件

语法：v-on:事件名称=“事件处理函数(参数)”

如何添加事件处理函数

1.定义事件处理函数的位置：methods属性

2.methods是一个单独的成员



**为什么事件处理函数需要在methods中添加****

1.这个结构中的this是指向当前组件对象

2.理论上讲，你也可以在data中定义，但是这里面的this指向null/window

3.data中只定义数据，不定义功能行为



**如何去传递事件参数**

1.如果你没有传递事件源对象，那么它也会默认的传递事件源对象

2.如果传递自定义参数，那么默认的事件源对象就不现传递啦

3.但是，如果传递自定义参数的同时，需要事件源对象，那么就需要手动传递$event

示例：

```vue
<button v-on:click='dosome("jack",$event)'>点我啊</button>
---------------------------------------------------------
methods: {
    dosome (name, event) {
      //   console.log('谢谢你点我')
      console.log(name)
      console.log(event)
    }
  }
```

简写：@

```vue
<button @click='dosome("jack",$event)'>点我啊</button>
```



**事件修饰符：**

1.prevent:阻止元素的默认行为

2.enter/13:键码和键别名

3.once:只触发一次

4.native:为组件的根元素添加原生事件

示例：

```vue
<div class="on">
    <p>这个文件用来说明v-on绑定事件</p>
    <p>{{msg}}</p>
    <!-- v-on:事件名称=“事件处理函数(参数)” -->
    <button v-on:click='dosome("jack",$event)'>点我啊</button>
    <button v-on:[type]='dothis'>你自由</button>
    <button @click='dosome("jack",$event)'>点我啊</button>
    <a href="http://baidu.com" @click.prevent.stop='sayHi'>单击我向你问好</a>
    <!-- <input type="text" v-model="key" @keydown.enter="showmsg"> -->
    <input type="text" v-model="key" @keydown.13="showmsg">
    <button @click.once='onceDo'>点我啊，只有一次机会</button>
    <button @click.once='key=123'>点我啊，只有一次机会</button>
</div>
```



#### v-if和v-show

这两个指令都能实现元素的显示和隐藏

语法：v-if='bool值'    v-show='bool值'

使用细节

1.v-if:是通过是否创建元素来决定元素是显示还是隐藏

2.v-show是通过样式的方式来决定元素是显示还是隐藏

**使用场景**

1.如果一个元素频繁的显示和隐藏，那么就使用v-show--可以节省创建元素的过程

2.如果元素的显示涉及到异步操作，就使用v-if--可以避免无用的数据占据内存空间

代码示例

```vue
<div class="show">
    <button @click="isShow = !isShow">单击切换显示隐藏</button>
    <p>这个文件主要讲解v-if和v-show实现元素的显示隐藏</p>
    <p>对于我们而言重要是掌握这两种方式实现显示隐藏的区别</p>
    <p v-if='isShow'>被控制的内容</p>
    <p v-show='isShow'>被控制的内容</p>
</div>
```



#### v-if和v-else-if和v-else

可以实现条件判断分支语句

细节：v-else-if 前面必须有v-if,v-else前面必须有v-if或v-else-if

示例：

```vue
<div class="score">
    <input type="number" v-model='score'>
	<p>这个案例使用v-if来实现成绩的展示</p>
    <p v-if='score>=90'>A</p>
    <p v-else-if='score>=80'>B</p>
    <p v-else-if='score>=70'>C</p>
    <p v-else-if='score>=60'>D</p>
    <p v-else>E</p>
</div>
```



#### v-cloak

v-cloak指令保持在元素上直到关联实例结束编译后自动移除，v-cloak和 CSS 规则如 [v-cloak] { display: none } 一起用时，这个指令可以隐藏未编译的 Mustache 标签直到实例准备完毕。
通常用来防止{{}}表达式闪烁问题

示例：

```vue
<style>
    [v-cloak] {
        display: none;
    }
</style>
-----------------
<p v-cloak>{{msg}}</p>
```

















