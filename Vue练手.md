# 一、后台管理系统 

课程视频： https://www.bilibili.com/video/av51530091?p=2

## 01、配置并使用 json-server

***一个在前端本地运行，可以存储json数据的server***

全局安装json-server

```
npm install -g json-server
```

初始化package.json文件 在JSONSERVER文件夹下 npm init

安装项目的json-server模块 在JSONSERVER文件夹下

```
npm install json-server --save
```

更改package.json的启动方式    在package.json文件中，修改代码

```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
修改为
"scripts": {
    "json:server": "json-server --watch db.json"
  },
```

在JSONSERVER文件夹下，创建db.json文件

```json
{
	"users": [
	{
		"name": "Henry",
		"phone": "333-444-555",
		"email": "henry@gmail.com",
		"id": 1,
		"age": 31,
		"companyId": 1
	},{
		"name": "Job",
		"phone": "333-444-555",
		"email": "job@gmail.com",
		"id": 2,
		"age": 32,
		"companyId": 2
	},{
		"name": "Bob",
		"phone": "333-444-555",
		"email": "bob@gmail.com",
		"id": 3,
		"age": 33,
		"companyId": 3
	},{
		"name": "Alice",
		"phone": "333-444-555",
		"email": "alice@gmail.com",
		"id": 4,
		"age": 34,
		"companyId": 1
	}],
	"companies": [
		{
			"id":1,
			"name":"Apple",
			"desc":"Apple is good!"
		},
		{
			"id":2,
			"name":"Honer",
			"desc":"Honer is best!"
		},
		{
			"id":3,
			"name":"OPPO",
			"desc":"OPPO is bad!"
		}
	]
}
```

打开cmd，启动json-server

```
json-server --watch db.json 或者 npm run json:server
```

使用json-server进行接口数据访问

```markdown
<!-- 获取所有用户信息 -->
http://localhost:3000/users

<!-- 获取指定id的用户信息 -->
http://localhost:3000/users/1

<!-- 获取所有公司信息 -->
http://localhost:3000/companies

<!-- 获取指定id的公司信息 -->
http://localhost:3000/companies/1

<!-- 获取指定id的公司的所有用户信息 -->
http://localhost:3000/companies/1/users

<!-- 根据公司名称获取公司信息 -->
http://localhost:3000/companies?name=Apple

<!-- 根据多个公司名称获取公司信息 -->
http://localhost:3000/companies?name=Apple&name=Honer

<!-- 分页查询 -->
http://localhost:3000/users?_page=1&_limit=2

<!-- 按照姓名进行升序排序 asc升序 desc降序 -->
http://localhost:3000/users?_sort=name&_order=asc

<!-- 获取年龄>=33的用户信息 -->
http://localhost:3000/users?age_gte=33

<!-- 获取年龄在32到33的用户信息 -->
http://localhost:3000/users?age_gte=32&age_lte=33

<!-- 模糊搜素 -->
http://localhost:3000/users?q=H
```

***附加***

将jsonplaceholder配置到json-server中，通过json-server访问jsonplaceholder中的数据

打开package.json文件

```json
"scripts": {
    "json:server": "json-server --watch db.json"
  },
添加
"scripts": {
    "json:server": "json-server --watch db.json",
    "json:server:remote": "json-server http://jsonplaceholder.typicode.com/db"
  },
```

重新启动

```
npm run json:server:remote
```



相关链接

https://www.jianshu.com/p/9cfc5cdb0aeb

https://github.com/typicode/json-server



## 02、搭建脚手架及导航

### 搭建脚手架

安装全局vue-cli脚手架

```
npm install --globle vue-cli
```

安装webpack创建vcustomers项目

```
vue init webpack vcustomers
```

注入依赖

```
cnpm install
```



### 创建导航

#### 导入路由router模块

- 1、注入router依赖

  ```
  cnpm install vue-router --save
  ```

- 2、在main.js文件中引入router，并注册路由

  ```javascript
  import VueRouter from "vue-router"
  
  Vue.use(VueRouter)
  ```

- 3、实例化VueRouter对象并配置路由

  ```javascript
  import Customers from "./components/Customers" //引入组件
  import About from "./components/About" //引入组件
  
  const router = new VueRouter({
      mode: 'history', //去掉url的#
      base: _dirname, //全局域名地址前缀
      routes: [
          { path: '/', component: Customers },
          { path: '/about', component: About }
      ]
  })
  ```

- 4、导出路由

  ```javascript
  new Vue({
      el: '#app',
      components: { App },
      router: router,
      template: '<App/>'
  })
  ```


#### 引入Bootstrap样式

进入bootCDN官网 https://www.bootcdn.cn/ ，复制3.37版本的bootstrap样式和js文件，粘贴到项目根路径的index.html中

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1.0" />
    <link href="https://cdn.bootcss.com/twitter-bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet" />
    <title>vcustomers</title>
</head>

<body>
    <div id="app"></div>
    <!-- built files will be auto injected -->
    <!-- jQuery (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
    <script src="https://cdn.jsdelivr.net/npm/jquery@1.12.4/dist/jquery.min.js"></script>
    <!-- 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
    <script src="https://cdn.bootcss.com/twitter-bootstrap/3.3.7/js/bootstrap.min.js"></script>
</body>

</html>
```

此时就可以进入bootstrap官网，使用组件进行布局



## 03、获取并展示用户信息

安装并引入 vue-resourse ，然后就可以使用 $http 访问json-server的接口

```
cnpm install vue-resource --save-dev
```

```javascript
import Vue from 'vue'
import App from './App'
import VueRouter from "vue-router" //引入路由插件
import VueResource from 'vue-resource' //引入资源请求插件
import Customers from "./components/Customers"
import About from "./components/About"

// 配置路由
Vue.use(VueRouter);
const router = new VueRouter({
    mode: 'history',
    base: __dirname,
    routes: [
        { path: '/', component: Customers },
        { path: '/about', component: About }
    ]
})

// 配置vue-resource
Vue.use(VueResource);
```

引入vue-resource后，可以基于全局的Vue对象使用http，也可以基于某个Vue实例使用http

```javascript
// 基于全局Vue对象使用http
Vue.http.get('/someUrl', [options]).then(successCallback, errorCallback);
Vue.http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);

// 在一个Vue实例内使用$http
this.$http.get('/someUrl', [options]).then(successCallback, errorCallback);
this.$http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);
```

```javascript
// 传统写法
this.$http.get('/Url', [options]).then(function(response){
    // 响应成功回调
}, function(response){
    // 响应错误回调
});

// ES6写法
this.$http.get('/Url', [options]).then((response) => {
    // 响应成功回调
}, (response) => {
    // 响应错误回调
});
```

##### 有7种符合REST风格的请求API：

```javascript
get(url, [options])
head(url, [options])
delete(url, [options])
jsonp(url, [options])
post(url, [body], [options])
put(url, [body], [options])
patch(url, [body], [options])
```



# 二、资金管理系统带权限

课程视频：https://www.bilibili.com/video/av59056478

*vue + elementUI + nodejs + mongdb*

1. **构建接口文档  Nodejs + express + jwt**
2. **构建前端页面  VueCli3.0 + ElementUI**
3. **数据请求及拦截  Axios + Mlab(数据存储) + MongoDB**(数据库)

![1567910156308](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\1567910156308.png)



## 01、express搭建服务器

在项目文件夹下新建node-demo/node-app文件夹，在node-app文件夹中初始化node

```
npm init
```

创建入口文件 server.js（在初始化时，修改index.js为server.js）

安装express

```
npm install express
```

在server.js文件中引入express

```javascript
const express = require("express"); //引入express
const app = express(); //实例化express

//设置路由
app.get('/', (req, res) => {
    res.send("Hello World");
});

const port = process.env.PORT || 5000; //设置端口

app.listen(port, () => {
    console.log(`Server is running on port ${port}`)
})
```

运行express

```
node server.js
```

使用nodemon，启动express（目的是方便修改文件后自动重启express）

```
npm install -g nodemon
nodemon server.js
```

在package.json中配置快捷命令

```json
{
  "name": "app",
  "version": "1.0.0",
  "description": "restful api",
  "main": "server.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node server.js",
    "server": "nodemon server.js"
  },
  "author": "Ankong",
  "license": "ISC"
}

```

访问本地服务器 localhost:5000

```
npm run server
```

## 02、连接在线的MongoDB数据库

在node-app文件下安装mongoose

```
npm install mongoose
```

在server.js文件引入和使用mongoose

```javascript
const express = require("express"); //引入express
const mongoose = require("mongoose"); //引入mongoose
const app = express(); //实例化express

// DB config
const db = require('./config/keys').mongoURI;

// Connect to mongodb
mongoose.connect(db, { useNewUrlParser: true })
    .then(() => console.log("MongoDB Connected"))
    .catch(err => console.log(err))

// 设置路由
app.get('/', (req, res) => {
    res.send("Hello World");
});

const port = process.env.PORT || 5000; //设置端口

app.listen(port, () => {
    console.log(`Server is running on port ${port}`)
})
```

新建数据库配置文件key.js

```javascript
module.exports = {
    mongoURI: "mongodb+srv://test:rak123456@ankongcluster01-d8sdf.mongodb.net/test?retryWrites=true&w=majority"
}
```

## 03、搭建路由和数据模型

routes   api























