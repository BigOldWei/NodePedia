# Node.js http 模块

HTTP 核心模块是 Node.js 网络的关键模块。

它可以使用

```js
const http = require('http');
```

该模块提供了一些属性和方法，以及一些类。

## 特性

### `http.METHODS`

此属性列出了所有支持的 HTTP 方法：

```console
> require('http').METHODS
[ 'ACL',
  'BIND',
  'CHECKOUT',
  'CONNECT',
  'COPY',
  'DELETE',
  'GET',
  'HEAD',
  'LINK',
  'LOCK',
  'M-SEARCH',
  'MERGE',
  'MKACTIVITY',
  'MKCALENDAR',
  'MKCOL',
  'MOVE',
  'NOTIFY',
  'OPTIONS',
  'PATCH',
  'POST',
  'PROPFIND',
  'PROPPATCH',
  'PURGE',
  'PUT',
  'REBIND',
  'REPORT',
  'SEARCH',
  'SUBSCRIBE',
  'TRACE',
  'UNBIND',
  'UNLINK',
  'UNLOCK',
  'UNSUBSCRIBE' ]
```

### `http.STATUS_CODES`

此属性列出所有 HTTP 状态代码及其描述：

```console
> require('http').STATUS_CODES
{ '100': 'Continue',
  '101': 'Switching Protocols',
  '102': 'Processing',
  '200': 'OK',
  '201': 'Created',
  '202': 'Accepted',
  '203': 'Non-Authoritative Information',
  '204': 'No Content',
  '205': 'Reset Content',
  '206': 'Partial Content',
  '207': 'Multi-Status',
  '208': 'Already Reported',
  '226': 'IM Used',
  '300': 'Multiple Choices',
  '301': 'Moved Permanently',
  '302': 'Found',
  '303': 'See Other',
  '304': 'Not Modified',
  '305': 'Use Proxy',
  '307': 'Temporary Redirect',
  '308': 'Permanent Redirect',
  '400': 'Bad Request',
  '401': 'Unauthorized',
  '402': 'Payment Required',
  '403': 'Forbidden',
  '404': 'Not Found',
  '405': 'Method Not Allowed',
  '406': 'Not Acceptable',
  '407': 'Proxy Authentication Required',
  '408': 'Request Timeout',
  '409': 'Conflict',
  '410': 'Gone',
  '411': 'Length Required',
  '412': 'Precondition Failed',
  '413': 'Payload Too Large',
  '414': 'URI Too Long',
  '415': 'Unsupported Media Type',
  '416': 'Range Not Satisfiable',
  '417': 'Expectation Failed',
  '418': 'I\'m a teapot',
  '421': 'Misdirected Request',
  '422': 'Unprocessable Entity',
  '423': 'Locked',
  '424': 'Failed Dependency',
  '425': 'Unordered Collection',
  '426': 'Upgrade Required',
  '428': 'Precondition Required',
  '429': 'Too Many Requests',
  '431': 'Request Header Fields Too Large',
  '451': 'Unavailable For Legal Reasons',
  '500': 'Internal Server Error',
  '501': 'Not Implemented',
  '502': 'Bad Gateway',
  '503': 'Service Unavailable',
  '504': 'Gateway Timeout',
  '505': 'HTTP Version Not Supported',
  '506': 'Variant Also Negotiates',
  '507': 'Insufficient Storage',
  '508': 'Loop Detected',
  '509': 'Bandwidth Limit Exceeded',
  '510': 'Not Extended',
  '511': 'Network Authentication Required' }
```

### `http.globalAgent`

指向 Agent 对象的全局实例，它是 `http.Agent` 类的一个实例。

它用于管理 HTTP 客户端的连接持久性和重用，它是 Node.js HTTP 网络的关键组件。

稍后会在 `http.Agent` 类描述中了解更多信息。

＃＃ 方法

### `http.createServer()`

返回 `http.Server` 类的新实例。

用法：

```js
JScopy
const server = http.createServer((req, res) => {
  // handle every single request with this callback
});
```

### `http.request()`

向服务器发出 HTTP 请求，创建 `http.ClientRequest` 类的实例。

### `http.get()`

类似于 `http.request()`，但自动将 HTTP 方法设置为 GET，并自动调用 `req.end()`。

## 类

HTTP 模块提供了 5 个类：

- `http.Agent`
- `http.ClientRequest`
- `http.Server`
- `http.ServerResponse`
- `http.IncomingMessage`

### `http.Agent`

Node.js 创建 `http.Agent` 类的全局实例来管理 HTTP 客户端的连接持久性和重用，这是 Node.js HTTP 网络的关键组件。

该对象确保向服务器发出的每个请求都排队并重用单个套接字。

它还维护一个套接字池。这是性能原因的关键。

### `http.ClientRequest`

调用 `http.request()` 或 `http.get()` 时会创建 `http.ClientRequest` 对象。

当收到响应时，会使用响应调用 `response` 事件，并使用 `http.IncomingMessage` 实例作为参数。

可以通过两种方式读取响应的返回数据：

- 你可以调用`response.read()`方法
- 在 `response` 事件处理程序中，您可以为 `data` 事件设置一个事件侦听器，以便您可以侦听流入的数据。

### `http.Server`

此类通常在使用`http.createServer()` 创建新服务器时实例化并返回。

一旦你有了一个服务器对象，你就可以访问它的方法：

- `close()` 阻止服务器接受新连接
- `listen()` 启动 HTTP 服务器并监听连接

### `http.ServerResponse`

由 `http.Server` 创建并作为第二个参数传递给它触发的 `request` 事件。

众所周知并在代码中用作`res`：

```js
const server = http.createServer((req, res) => {
  // res is an http.ServerResponse object
});
```

您将始终在处理程序中调用的方法是 `end()`，它关闭响应，消息完成，服务器可以将其发送给客户端。必须在每个响应上调用它。

这些方法用于与 HTTP 标头交互：

- `getHeaderNames()` 获取已设置的 HTTP 标头的名称列表
- `getHeaders()` 获取已设置的 HTTP 标头的副本
- `setHeader('headername', value)` 设置 HTTP 标头值
- `getHeader('headername')` 获取已设置的 HTTP 标头
- `removeHeader('headername')` 删除已设置的 HTTP 标头
- `hasHeader('headername')` 如果响应设置了该标头，则返回 true
- 如果标头已经发送到客户端，则`headersSent()`返回true

处理完标头后，您可以通过调用 `response.writeHead()` 将它们发送到客户端，它接受 statusCode 作为第一个参数、可选的状态消息和标头对象。

要在响应正文中向客户端发送数据，请使用`write()`。它将缓冲数据发送到 HTTP 响应流。

如果尚未使用 `response.writeHead()` 发送标头，它将首先发送标头，以及在请求中设置的状态代码和消息，您可以通过设置 `statusCode` 和 `statusMessage` 属性进行编辑价值观：

```js
response.statusCode = 500;
response.statusMessage = 'Internal Server Error';
```

### `http.IncomingMessage`

`http.IncomingMessage` 对象由以下方式创建：

- 监听 `request` 事件时的`http.Server`
- 监听 `response` 事件时的 `http.ClientRequest`

它可用于访问响应：

- 使用其 `statusCode` 和 `statusMessage` 方法的状态
- 使用其 `headers` 方法或 `rawHeaders` 的标题
- 使用其 `method` 方法的 HTTP 方法
- 使用 `httpVersion` 方法的 HTTP 版本
- 使用 `url` 方法的 URL
- 使用 `socket` 方法的底层套接字

使用流访问数据，因为 `http.IncomingMessage` 实现了可读流接口。
