https://www.cnblogs.com/AhuntSun-blog/p/12529920.html



Cache-Control：设置静态资源缓存时间

![image-20200304164341625](C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200304164341625.png)

**传输层：**

1、向用户提供可靠的端到端的服务（End-to-End）

2、传输层向高层屏蔽了下层数据通信的细节

**一个TCP连接对应多个HTTP请求，一个HTTP请求在一个TCP连接之上。**

## HTTP发展历史

https://www.cnblogs.com/aspirant/p/10833143.html

### HTTP/0.9

- 只有一个命令GET
- 没有HEADER等描述数据的信息
- 服务器发送完毕，就关闭TCP连接，一个TCP连接上只有一个HTTP请求

### HTTP/1.0

增加了多个请求命令： OPTIONS、GET、HEAD、POST、PUT、DELETE、TRACE、CONNECT

增加了status code 和 header

多字符集支持、多部分发送、权限、缓存等，一个TCP连接上有多个HTTP请求，但是是按顺序发送，而不是并行的

### HTTP/1.1

持久连接，一个TCP连接上有多个HTTP请求，且是并行发送

pipeline

增加host和其他一些命令

### HTTP2

所有数据以二进制传输

同一个连接里面发送多个请求不再需要按照顺序来

头信息压缩以及推送等提高效率的功能

### HTTPs

## HTTP三次握手

<img src="C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200304220513730.png" alt="image-20200304220513730" style="zoom: 25%;" />

防止服务器开启无用的连接，浪费资源

第一次：客户端告诉服务端我将要发送请求

第二次：服务器告诉客户端，我准备接受了，你可以发送了

第三次：客户端告诉服务器，我马上就发

## URI  URL  URN

URI：Uniform Resource Indentifier 统一资源标志符   用来唯一标识互联网上的信息资源，包含了URL和URN

URL：Uniform Resource Locator 统一资源定位器  http://user:pass@host.com:80/path?query=string#hash

URN：永久统一资源定位符   在资源移动之后还能被找到，目前还没有非常成熟的使用方案

## HTTP报文格式





## 实现HTTP

nodeJS 实现服务端

```javascript
const http = require('http');

http.createServer(function(request, response) {
  console.log('request come', request.url)
  response.end('123')
}).listen(8888)

console.log('创建成功，在浏览器访问localhost:8888')
```

http客户端

最简单的http客户端就是浏览器，curl也是http客户端

使用gitBash命令行执行curl命令，查看http响应报文

```bash
curl baidu.com

curl www.baidu.com

curl -v www.baidu.com
```

## CORS跨域请求的限制与解决

https://segmentfault.com/a/1190000011145364

跨域问题是由于浏览器（客户端）不允许接收非同源url的响应导致的。实际上对于非同源的服务器来说，它在接收到http请求后，正常返回响应报文，但是浏览器拦截了非同源服务器的响应并报错。













