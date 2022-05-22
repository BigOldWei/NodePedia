# 使用 Node.js 获取 HTTP 请求正文数据

以下是如何提取请求正文中作为 JSON 发送的数据的方法。

如果您使用 Express，那非常简单：使用 Express v4.16.0 及更高版本中可用的 `express.json()` 中间件。

例如，要获取此请求的正文：

```js
const axios = require('axios');

axios.post('https://whatever.com/todos', {
  todo: 'Buy the milk',
});
```

这是匹配的服务器端代码：

```js
const express = require('express');

const app = express();

app.use(
  express.urlencoded({
    extended: true,
  })
);

app.use(express.json());

app.post('/todos', (req, res) => {
  console.log(req.body.todo);
});

```

如果您不使用 Express，并且想在普通 Node.js 中执行此操作，那么您当然需要做更多的工作，因为 Express 为您抽象了很多这些内容。

要理解的关键是，当您使用`http.createServer()` 初始化 HTTP 服务器时，当服务器获取所有 HTTP 标头而不是请求正文时会调用回调。

连接回调中传递的 `request` 对象是一个流。

所以，我们必须监听要处理的正文内容，并且是分块处理的。

我们首先通过监听流`data`事件来获取数据，当数据结束时，会调用流`end`事件，一次：

```js
const server = http.createServer((req, res) => {
  // we can access HTTP headers
  req.on('data', chunk => {
    console.log(`Data chunk available: ${chunk}`);
  });
  req.on('end', () => {
    // end of data
  });
});
```

所以要访问数据，假设我们期望接收一个字符串，我们必须在监听流`data`时将块连接成一个字符串，当流`end`时，我们将字符串解析为JSON：

```js
const server = http.createServer((req, res) => {
  let data = '';
  req.on('data', chunk => {
    data += chunk;
  });
  req.on('end', () => {
    console.log(JSON.parse(data).todo); // 'Buy the milk'
    res.end();
  });
});

```

从 Node.js v10 开始，可以使用“for await .. of”语法。 它简化了上面的例子，使它看起来更线性：

```js
const server = http.createServer(async (req, res) => {
  const buffers = [];

  for await (const chunk of req) {
    buffers.push(chunk);
  }

  const data = Buffer.concat(buffers).toString();

  console.log(JSON.parse(data).todo); // 'Buy the milk'
  res.end();
});
```
