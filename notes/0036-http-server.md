# 构建 HTTP 服务器

这是一个示例 Hello World HTTP Web 服务器：

```js
const http = require('http')

const port = process.env.PORT || 3000

const server = http.createServer((req, res) => {
  res.statusCode = 200
  res.setHeader('Content-Type', 'text/html')
  res.end('<h1>Hello, World!</h1>')
})

server.listen(port, () => {
  console.log(`Server running at port ${port}`)
})
```

让我们简要分析一下。 我们包含了 [`http` 模块](https://nodejs.org/api/http.html)。

我们使用该模块来创建一个 HTTP 服务器。

服务器设置为侦听指定端口“3000”。 当服务器准备好时，会调用 `listen` 回调函数。

我们传递的回调函数将在每个请求进入时执行。每当收到新请求时，[`request` 事件](https://nodejs.org/api/http.html#http_event_request ) 被调用，提供两个对象：一个请求（一个 [`http.IncomingMessage`](https://nodejs.org/api/http.html#http_class_http_incomingmessage) 对象）和一个响应（一个 [`http.ServerResponse`] （https://nodejs.org/api/http.html#http_class_http_serverresponse）对象）。

`request` 提供请求详细信息。 通过它，我们访问请求头和请求数据。

`response` 用于填充我们要返回给客户端的数据。

在这种情况下

```js
res.statusCode = 200;
```

我们将 statusCode 属性设置为 200，表示响应成功。

我们还设置了 Content-Type 标头：

```js
res.setHeader('Content-Type', 'text/html');
```

然后我们结束关闭响应，将内容作为参数添加到 `end()`：

```js
res.end('<h1>Hello, World!</h1>');
```
