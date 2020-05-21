

# Webpack4.0

[概念 | webpack 中文网](https://www.webpackjs.com/concepts/)

**模块打包器，分析依赖关系**

### 4.0新特性

**mode属性**

webpack需要设置mode属性，可以是development   production

<img src="C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200319002550473.png" alt="image-20200319002550473" style="zoom: 25%;" />

**插件和优化**

<img src="C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200319002738690.png" alt="image-20200319002738690" style="zoom:25%;" />

**开箱即用WebAssembly**

<img src="C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200319002833672.png" alt="image-20200319002833672" style="zoom: 25%;" />

**0C JS**

<img src="C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200319003049778.png" alt="image-20200319003049778" style="zoom:25%;" />

**新的插件**

<img src="C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200319003259183.png" alt="image-20200319003259183" style="zoom:25%;" />

使用webpack4.0确保Node.js的版本>=8.9.4，由于webpack4使用了很多JS新的语法，它们在新版本的v8里面进过了优化。



**安装webpack命令行工具（脚手架）**

```shell
npm install webpack-cli -g
```

**打包命令**

```shell
#如果没有配置文件，需要加上mode
webpack --mode development <入口文件.js> -o <出口文件.js> 

#如果有配置文件
webpack
```



**出口入口配置** **webpack.config.js**

```javascript
const path = require('path');

// module.exports = {
//   entry: './index.js', // 入口文件
//   output: { // 出口文件
//     path: path.resolve(__dirname, 'dist'),
//     filename: 'output.bundle.js'
//   },
//   mode: 'development' // 模式
// }

module.exports = {
  entry: { // 入口文件 可以是一个对象，多个入口文件
    home: './home.js',
    about: './about.js',
    other: './other.js'
  },
  output: { // 出口文件
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].bundle.js' // [name]指代入口文件的key
  },
  mode: 'development' // 模式
}
```



**加载器 Loaders**

https://www.webpackjs.com/concepts/loaders/

https://www.webpackjs.com/loaders/url-loader/

安装loader

```shell
#先生成package.json文件
npm init
#再安装加载器
npm install --save-dev <loader>
```

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 8192
            }
          }
        ]
      }
    ]
  }
}
```

rules：是对文件扩展名进行过滤，过滤依据是test

例：**url-loader**是将图片文件转成base64的加载器，使用它之前要先安装file-loader

**babel-loader**是负责将es6的语法转成es5语法的加载器

```shell
npm install babel-loader@8.0.0-beta.0 @babel/core @babel/preset-env webpack
```



**插件Plugins**

 

**热替换DevServer** 



## 一、Webpack 初探

支持ES Module和CommonJS等其他模块引入规范

起初只能打包js文件，现在可以打包任何类型文件

https://webpack.js.org/api/

### 1、安装webpack

**安装node -> 初始化npm,生成package.json -> 安装webpack和webpack-cli**

<u>高版本的node和webpack会提升打包的速度</u>

```shell
# -y表示使用默认配置
npm init -y
```



```shell
npm install webpack webpack-cli -g
```

**全局安装webpack的弊端**：全局只有一个webpack版本，对于使用了不同版本webpack的多个项目会出现无法运行的问题。

```shell
#卸载全局webpack和webpack-cli
npm uninstall webpack webpack-cli -g
```

------

**webpack-cli**：webpack脚手架，使我们能够在命令行中使用webpack指令



在做开发时，最好是在项目中安装webpack和webpack-cli

```shell
npm install webpack webpack-cli --save-dev
# --save-dev 可以直接改为 -D
```

**注意：**当安装局部的webpack时，无法直接在命令行中使用webpack指令，因为命令行中webpack指令默认是从全局的node_modules文件中找webpack，此时需要通过使用npm提供的 **npx** 指令来使用项目中node_modules文件中的webpack

```shell
npx webpack -v
```

 

安装指定版本的webpack

```shell
#查看webpack的所有历史版本信息
npm view webpack versions

#安装指定版本的webpack
npm install webpack@4.16.5 webpack-cli -D
```



### 2、webpack配置文件

新建**webpack.config.js**文件

```shell
npx webpack
```

这是默认的webpack配置文件名，如果更改了配置文件名，比如改为webconfig.js，则此时要指定配置文件

```shell
npx webpack --config webconfig.js
```

**没事不要改这个名字！**



```javascript
const path = require('path');

module.exports = {
  entry: './src/index.js',
  /**
  等价于
  entry: {
	main: './src/index.js'
  },
  */ 
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  mode: 'development'
}
```



每次在命令行输入npx webpack有点麻烦，所以我们在npm的配置文件(package.json)中添加script命令

```json
"scripts": {
    "bundle": "webpack"
},
```

**注意**：此处直接写webpack而不是npx webpack原因是，在package中配置了script命令，默认寻找的是项目中node_modules文件中的webpack，而非全局的node_modules文件中的webpack，所以不用写npx。



此时，再进行打包时，直接使用命令

```shell
npm run bundle
```

 

### 3、打包输出内容

![image-20200406215425176](C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200406215425176.png)

**mode**默认是production，打包出来的文件是删除了空格等字符和注释，开发时用development



## 二、webpack核心概念

### 1、Loader加载器

**webpack默认只能打包js文件模块**，此时如果要打包图片等其他格式的文件，需要在配置里面指定module.rules，数组类型，使用加载器 **file-loader**

https://webpack.docschina.org/loaders/file-loader/

**先安装loader -> 在webapck.config.js文件中添加module.rules -> 执行打包**

```shell
npm install file-loader -D
```

```javascript
const path = require('path');

module.exports = {
  entry: './src/index.js',
  module: {
    rules: [{
      test: /\.jpg$/, //文件类型配置
      use: {
        loader: 'file-loader'
      }
    }]
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  mode: 'development'
}
```

```shell
npm run bundle
```

![image-20200406225025087](C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200406225025087.png)



**file-loader做了什么**：将入口文件中关于图片依赖的文件复制到dist文件夹中，并修改文件名，再将文件名分装到一个Module对象中返回给入口文件中。

<img src="C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200406232355719.png" alt="image-20200406232355719" style="zoom: 50%;" />

file-loader不只是能打包图片文件，它能打包任何格式文件，前提是在**test**中做文件类型的配置。



**Loader就是一种打包方案**



#### 使用Loader打包静态资源——图片

```javascript
const path = require('path');

module.exports = {
  entry: './src/index.js',
  module: {
    rules: [{
      test: /\.(jpg|png|gif)$/,
      use: {
        loader: 'file-loader',
        options: {
          // 重命名文件
          name: '[name]_[hash].[ext]',
          // 图片打包到dist/images文件夹中
          outputPath: 'images/'
        }
      }
    }]
  },
  output: {
    // 打包文件的根目录文件夹
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  mode: 'development'
}
```



https://webpack.docschina.org/loaders/url-loader/

**url-loader**：可以实现file-loader所有功能，并且针对图片文件，**它有两种打包机制**，一种是将图片文件转成base64编码存放到bundle.js文件中，另一种和file-loader一致，生成图片文件。

两种图片打包机制的区别，一种是转成base64存在bundle.js文件中，js文件加载了就等于加载了图片，但此时js文件会很大；另一种是生成图片文件，在浏览器渲染时需要通过http请求本地图片文件。

通过在url-loader的options中添加 **limit** 字段中设置最大的图片大小，超过这个值，图片打包采用生成图片的机制，低于这个值，图片打包采用base64机制。

```javascript
const path = require('path');

module.exports = {
  entry: './src/index.js',
  module: {
    rules: [{
      test: /\.(jpg|png|gif)$/,
      use: {
        loader: 'url-loader',
        options: {
          // 重命名文件，[xxx]占位符
          name: '[name]_[hash].[ext]',
          // 图片打包到dist/images文件夹中
          outputPath: 'images/',
          // 设置图片打包机制的分界值，2kb
          limit: 2048
        }
      }
    }]
  },
  output: {
    // 打包文件的根目录文件夹
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  mode: 'development'
}
```



#### 使用Loader打包静态资源——样式CSS

**style-loader  css-loader**

```shell
npm install style-loader css-loader -D
```

```javascript
module: {
    rules: [{
      test: /\.css$/,
      use: ['style-loader', 'css-loader']
    }]
  },
```



**样式打包原理**：首先是css-loader分析所有css文件的依赖关系(比如 @import 语法引入的css文件)，然后整合成一个css规则表，之后再由style-loader将这个css挂载到页面的head标签中的style标签中。

<img src="C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200407005139618.png" alt="image-20200407005139618" style="zoom: 50%;" />



如果我们要对.scss这样的样式文件打包，需要再引入sass-loader，同理stylus，less也是要再引入相应的loader

```javascript
test: /\.scss$/,
use: ['style-loader', 'css-loader', 'sass-loader']
```

当引入多个loader时，执行顺序是 **从后往前** ，先执行sass-loader，将scss文件转成css文件，再执行css-loader，最后执行style-loader，所以loader的顺序不能出错。



 厂商前缀：**postcss-loader**



##### **模块化css**

解决的问题是全局css样式会影响到其他文件的样式，导致样式冲突。

CSS Module是指当前css文件只在当前的模块中起作用。



**css-loader的配置项**

```javascript
test: /\.scss$/,
use: ['style-loader', {
        loader: 'css-loader',
        options: {
          importLoaders: 2,
          modules: true
        }
      }, 'sass-loader', 'postcss-loader']
```

**importLoaders**：当解析一个scss文件时，先走sass-loader再走css-loader，如果某个scss文件内部又引入了scss文件，此时不会返回sass-loader再走一遍，为了能够从前面的loader开始再遍历一遍，设置**importLoaders**，表示从前面几个重新开始遍历loader

**modules**：true  开启css模块化后，引入样式文件通过 import style from 'xxx.css'这种方式，之前未开启css模块化，直接通过 import 'xxx.css' 的方式即可引入样式文件。

```javascript
import style from './index.scss'
style.选择器
```



#### 使用Loader打包静态资源——字体图标文件iconfont

[https://webpack.docschina.org/guides/asset-management/#%E5%8A%A0%E8%BD%BD-fonts-%E5%AD%97%E4%BD%93](https://webpack.docschina.org/guides/asset-management/#加载-fonts-字体)

针对字体文件（后缀名eot、svg、ttf、woff等），借助**file-loader**进行文件拷贝即可

```javascript
test: /\.(svg|eot|ttf|woff)$/,
      use: {
        loader: 'file-loader'
      }
```



### 2、Plugin插件

loader 用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。包括：打包优化，资源管理，注入环境变量。插件目的在于解决 [loader](https://webpack.docschina.org/concepts/loaders) 无法实现的其他事。



**先安装Plugin -> 在webapck.config.js文件中引入plugin -> 在与module同级添加plugins属性 ->执行打包**



plugins属性和rules属性一样，值为数组类型，插件可以在打包前或者打包后运行



**html-webpack-plugin**：在打包之后，自动在dist文件下生成index.html文件，并且自动引入打包生成的bundle.js

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  module: {
    rules: []
  },
  plugins: [new HtmlWebpackPlugin()],
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  mode: 'development'
}
```

此时生成的index.html文件是没有任何元素的，我们可以给插件提供一个html的模板，重新打包后就会发现，在dist文件下生成的index.html文件与我们提供的html模板文件相比，只是添加了一个script标签将bundle.js文件引入其中。（如果有多个入口文件，也会全部引入）

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  module: {
    rules: []
  },
  plugins: [new HtmlWebpackPlugin({
	template: 'src/index.html'
  })],
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  mode: 'development'
}
```



**clean-webpack-plugin**：当我们每次重新打包时，实际上是不会自动删除之前生成的dist文件夹，这时候如果我们更改了output文件名，就会在dist文件中留下之前打包的js文件，为了解决这一问题，clean-webpack-plugin会在每次重新打包时，删除dist文件夹（output.path）。

*这个插件是非官方插件

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  module: {
    rules: []
  },
  plugins: [new HtmlWebpackPlugin({
    template: 'src/index.html'
  }), new CleanWebpackPlugin()],
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  mode: 'development'
}
```



### 3、Entry入口/Output出口配置

```javascript
entry: './src/index.js' // 指定打包的入口文件
// 等价于
entry: {
   main: './src/index.js'
}
```



针对**多个入口文件的打包**，打包后出口文件数目要和入口文件数目一致，一般我们用占位符[name].js

```javascript
entry: {
   main: './src/index.js',
   sub: './src/sub.js'
},
output: {
   filename: '[name].js',
   path: path.resolve(__dirname, 'dist')
}
```



有些情况下，我们会将dist文件夹下的js文件上传到某个cdn服务器上，此时index.html文件中的script标签src地址就不再是bundle.js这样的相对路径，而是加上cdn服务器的地址。

在output中配置**publicPath**

```javascript
output: {
   publicPath: 'http://cdn.com.cn',
   filename: '[name].js',
   path: path.resolve(__dirname, 'dist')
}
```



### 4、sourceMap

**sourceMap**是一个映射关系，它知道dist目录下打包的js文件的每一行代码对应的src目录下的源码。例如，如果sourceMap关闭，当浏览器报错时，它只是指向打包后的js文件，而不是指向打包前的源码，对我们调试代码不友好。



在mode：'development'模式下，sourceMap默认打开，配置devtool：'none'，可以关闭sourceMap

```javascript
mode: 'development',
    
// 关闭sourceMap
devtool: 'none'

// 打开sourceMap，此时打包会生成bundle.js.map文件
devtool: 'source-map'

// 打开sourceMap，此时不会生成bundle.js.map文件，而是将其通过data-url的方式写在bundle.js文件中
devtool: 'inline-source-map'

// 打开sourceMap，此时不会生成bundle.js.map文件，而是将其通过data-url的方式写在bundle.js文件中，相比于inline-source-map性能会更高，因为它只定位到行，不会具体到某个单词
devtool: 'cheap-inline-source-map'

// 打开sourceMap，打包速度最快，但是提示内容可能不全面
devtool: 'eval'
```

在**开发**时，建议设置

```javascript
mode: 'development',
devtool: 'cheap-module-eval-source-map'
```

在**生产**时，建议设置

```javascript
mode: 'production',
devtool: 'cheap-module-source-map'
```



### WebpackDevServer

优化1：监听源代码，只要源代码发生改动，webpack自动打包，无需手动webpack

```json
"scripts": {
    "watch": "webpack --watch"
},
```



优化2：不仅可以自动打包，而且浏览器自动刷新，无需手动刷新页面，此时要开启**devServer**

首先要安装**webpack-dev-server**

```shell
npm install webpack-dev-server -D
```

```javascript
module.exports = {
  entry: './src/index.js',
  devtool: 'cheap-module-eval-source-map',
  devServer: {
    contentBase: './dist'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  mode: 'development'
}
```

```json
"scripts": {
  "start": "webpack-dev-server"
},
```

**此时开起了web服务器，并且不再生成dist文件夹，此时打包的文件实际上不是存在磁盘中，而是存在内存中**



优化3：**自动打开浏览器**

```javascript
devServer: {
  contentBase: './dist',
  open: true, // 自动打开浏览器
},
```



devServer其他配置

```javascript
devServer: {
  contentBase: './dist',
  open: true,
  port: 3000, // 更改默认端口号
  proxy: {
	'/api': 'http://www.baidu.com' // 跨域
  }
},
```



### HMR (hot module replacement) 热模块更新

有这样的一个场景，页面中有些元素是我们动态生成的，当我们修改css文件之后，由于开启了WebpackDevServer，它会自动打包并且刷新页面，那么这些动态添加的元素就会消失，需要我们重新触发事件，我们希望对于这些动态添加的元素，在我们修改css样式之后，页面不会刷新并且新的样式也会生效。

**引入webpack -> 配置webpack.HotModuleReplacementPlugin -> 配置devServer**

```javascript
const webpack = require('webpack');

module.exports = {
  entry: './src/index.js',
  devtool: 'cheap-module-eval-source-map',
  devServer: {
    contentBase: './dist',
    open: true,
    hot: true, // 开启HMR
    hotOnly: true // 即便hot未生效，也不让页面刷新
  },
  module: {
    rules: []
  },
  plugins: [new webpack.HotModuleReplacementPlugin()],
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  mode: 'development'
}

```

```javascript
devServer: {
    contentBase: './dist',
    open: true,
    hot: true, // 开启HMR
    hotOnly: true // 即便hot未生效，也不让页面刷新
  },
```

**hotOnly**作用是即便**hot**未生效，也不让页面刷新



针对js文件的热更新，需要写下面的代码让单个js文件热更新，不会影响到其它文件。

```javascript
import aJS from './aJS';
import bJS from './bJS';

aJS();
bJS();

// 只对aJS进行热更新
if(module.hot) {
    module.hot.accept('./aJS', () => {
        aJS();
    })
}
```

那么为什么对于css样式文件不需要这样写呢，因为css-loader已经帮我们写了这部分的代码。

























