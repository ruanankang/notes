

# 01、初识 NodeJS

## 1、什么是NodeJS

#### 前端最新主流JavaScript运行环境

- Node.js 是使用C++编写的；
- Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境；
- Node.js 使用了一个**事件驱动**、**非阻塞式 I/O 的模型**(通过回调函数实现)，使其轻量又高效；
- Node.js 的包管理器 npm，是全球最大的开源库生态系统；
- 工作原理：V8引擎、模块、事件队列、文件系统

下载安装  http://nodejs.cn/download/

```shell
node -v  或 node --version  //查看node版本
npm -v  或 npm --version  //查看npm版本
```

在命令行中执行javascript代码（node环境）

```shell
node //cmd中输入node即可切换至node环境
console.log("dsfg")
```

使用命令行执行app.js文件：在app.js文件夹目录中执行命令行

```shell
node app.js
```

## 2、V8引擎

浏览器中全局对象是window，但在node环境中，全局对象是 global  所以在node环境下，this指向为global

V8引擎是C++编写的

## 3、Module & Require

在 Node.js 中，文件和模块是一一对应的（每个文件被视为一个独立的模块）

http://nodejs.cn/api/modules.html

例如，假设有一个名为 `foo.js` 的文件：

```javascript
const circle = require('./circle.js');
console.log(`半径为 4 的圆的面积是 ${circle.area(4)}`);
```

在第一行中， `foo.js` 加载了与 `foo.js` 在同一目录中的 `circle.js` 模块。

以下是 `circle.js` 的内容：

```javascript
const { PI } = Math;

exports.area = (r) => PI * r ** 2;

exports.circumference = (r) => 2 * PI * r;
```

`circle.js` 模块导出了 `area()` 和 `circumference()` 函数。 通过在特殊的 `exports` 对象上指定额外的属性，可以将函数和对象添加到模块的根部。

模块内的本地变量是私有的，因为模块由 Node.js 封装在一个函数中（详见[模块封装器](http://nodejs.cn/s/rrmTGc)）。 在这个例子中，变量 `PI` 对 `circle.js` 是私有的。

可以为 `module.exports` 属性分配新的值（例如函数或对象）。

下面的例子中， `bar.js` 使用了导出 Square 类的 `square` 模块：

```javascript
const Square = require('./square.js');
const mySquare = new Square(2);
console.log(`mySquare 的面积是 ${mySquare.area()}`);
```

`square` 模块定义在 `square.js` 中：

```javascript
// 赋值给 `exports` 不会修改模块，必须使用 `module.exports`。
module.exports = class Square {
  constructor(width) {
    this.width = width;
  }

  area() {
    return this.width ** 2;
  }
};
```

模块系统在 `require('module')` 模块中实现。

## 4、事件模块 Events

http://nodejs.cn/api/events.html#events_asynchronous_vs_synchronous

- 大多数 Node.js 核心 API 都是采用惯用的**异步事件驱动架构（fs / http）；**
- 所有能触发事件的对象都是 EventEmitter 类的实例；
- **事件流程**：引入事件模块 -> 创建 EventEmitter对象 -> 注册事件 -> 触发事件

```javascript
// 1、引入系统事件模块，不用写路径，引入自定义模块需要写文件路径
var events = require('events');

// 2、创建 EventEmitter 对象
var myEmitter = new events.EventEmitter();

// 3、注册事件
myEmitter.on('someEvent', function(message) {
    console.log(message);
})

// 4、触发事件，可以传参数
myEmitter.emit('someEvent', '传递的参数');
```

```javascript
/**
 * 异步执行
 */
// 3、注册事件
myEmitter.on('someEvent', function(message) {
  setImmediate(() => {
    console.log(message);
  });
})

// 4、触发事件，可以传参数
myEmitter.emit('someEvent', '传递的参数');
```

## 5、文件系统模块 fs

http://nodejs.cn/api/fs.html

#### 文件的读取和写入

- 读取文件：**fs.readFile**
- 写入文件：**fs.writeFile**
- 流程：引入文件模块 -> 调用方法 -> 捕获异常

```javascript
// 文件系统

// 1. 引入文件系统模块
const fs = require('fs');

// 2. 通过对象调用方法
/**
 * 读取：同步方法readFileSync，异步方法readFile
 * 写入：同步方法writeFileSync，异步方法writeFile
 * 创建文件夹：同步方法mkdirSync，异步方法mkdir
 * 删除文件夹：同步方法rmdirSync，异步方法rmdir
 * 删除文件：unlink
 */
const response = fs.readFileSync('./03readMe.txt', 'utf8');
console.log(response);
// fs.writeFileSync('./03writeMe.txt', response); // 第二个参数可以是字符串等数据，也可以是文件流

fs.readFile('./03readMe.txt', 'utf8', (err, data) => {
    if (err) throw err;
    console.log(data);
  })
  // fs.writeFile('./03writeMe.txt', response);



// fs.unlink('./03readMe.txt', err => {
//   if (err) throw err;
//   console.log('文件删除成功')
// })
```



## 6、HTTP创建服务器

http://nodejs.cn/api/http.html

```javascript
// 通过http模块，创建本地服务器 ip 127.0.0.1  port 8888
const http = require('http')

// 创建服务器方法
const server = http.createServer(function(request, response) {
  console.log('客户端向服务器发送请求：', request.url);
  response.writeHead(200, {
    "Content-type": 'text/plain'
  })
  response.end('Server is working!')
})

// 服务对象监听服务器地址以及端口号
server.listen(8888, "127.0.0.1");

console.log("服务器正在运行")
```



## 7、缓存区及流

http://nodejs.cn/api/buffer.html

http://nodejs.cn/api/stream.html

缓存区：可以在TCP流和文件系统操作等场景中处理二进制数据流

流：处理流数据的抽象接口

管道事件

**创建流之后，只能使用一次**

```javascript
const fs = require('fs');
const http = require('http')

// 读取文件流
let myReadStream = fs.createReadStream(`${__dirname}/05readMe.txt`, 'utf8');
// 写入文件流
let myWriteStream = fs.createWriteStream(`${__dirname}/05writeMe.txt`)

// myReadStream.on('data', function(chunk) {
//   console.log('======正在接收一部分数据======');
//   // console.log(chunk);
//   myWriteStream.write(chunk);
// })

// 利用管道事件一次性进行文件流的读写 pipe() 输入流.pipe(输出流)
// myReadStream.pipe(myWriteStream);

// 在浏览器中显示文档
const server = http.createServer((req, res) => {
  console.log('浏览器向服务端发送请求：', req.url);
  res.writeHead(200, {
    'Content-type': 'text/plain'
  })
  myReadStream.pipe(res);
});

server.listen(8888, '127.0.0.1');
```



## 8、实现路由效果

根据不同的url路径访问页面

```javascript
const http = require('http');
const fs = require('fs');

// 创建服务器
http.createServer((req, res) => {
  if (req.url !== '/favicon.ico') {
    if (req.url === '/' || req.url === '/index') {
      res.writeHead(200, {
        'Content-type': 'text/html'
      })
      fs.createReadStream(`${__dirname}/06index.html`).pipe(res);
    } else if (req.url === '/contact') {
      res.writeHead(200, {
        'Content-type': 'text/html'
      })
      fs.createReadStream(`${__dirname}/06contact.html`).pipe(res);
    } else if (req.url === '/api') {
      res.writeHead(200, {
        'Content-type': 'application/json'
      })
      fs.createReadStream(`${__dirname}/06api.json`).pipe(res);
    }
  }
}).listen(8888);
```



## NPM

NPM是随同NodeJS一起安装的包管理工具，能解决很多NodeJS代码部署上的问题。

1、允许用户从NPM服务器下载别人编写的第三方包到本地使用；

2、允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用；

3、允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

**先初始化npm，生成package.json文件**

```shell
npm init

npm install <moudule name>

npm uninstall <moudule name>

npm install <moudule name> --save

npm install <moudule name> --save-dev
```



## Express框架

基于NodeJS平台，快速、开放、极简的web开发框架

https://www.expressjs.com.cn/starter/installing.html

```shell
npm install express --save-dev
```

Express框架已经封装好**服务器、路由、中间件、网络请求**。

```javascript
// 引入express模块
const express = require('express');

// 实例化express对象
let app = express();

// 根据用户请求的地址，返回对应的数据信息
app.get('/', (req, res) => {
  console.log(req.url);
  res.send('This is Home Page!');
});
app.get('/contact', (req, res) => {
  console.log(req.url);
  res.send('This is Contact Page!');
});

// 路由参数设定
app.get('/profile/:id', (req, res) => {
  console.log(req.url);
  res.send(`当前访问的profile的id为${req.params.id}`);
});

// 监听服务器端口号
app.listen(8888, '127.0.0.1')
```



在编写调试Node.js项目，修改代码后，需要频繁的手动close掉，然后再重新启动，非常繁琐。现在，我们可以使用`nodemon`这个工具，它的作用是监听代码文件的变动，当代码改变之后，自动重启。

```shell
npm install -g nodemon  #全局安装

npm install nodemon --save-dev  #项目中安装
```

使用**nodemon**替代node命令

```shell
nodemon <file>
```

重启服务

```shell
rs
```



### EJS模板引擎

```shell
npm install ejs --save-dev
```









