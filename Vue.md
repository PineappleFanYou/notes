

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

