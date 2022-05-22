# 使用 Node.js 发出 HTTP 请求

## 执行 GET 请求

在 Node.js 中执行 HTTP GET 请求的方法有很多种，具体取决于您要使用的抽象级别。

使用 Node.js 执行 HTTP 请求的最简单方法是使用 [Axios 库](https://github.com/axios/axios)：

```js
const axios = require('axios');

axios
  .get('https://example.com/todos')
  .then(res => {
    console.log(`statusCode: ${res.status}`);
    console.log(res);
  })
  .catch(error => {
    console.error(error);
  });
```

但是，Axios 需要使用 3rd 方库。

仅使用 Node.js 标准模块就可以进行 GET 请求，尽管它比上面的选项更详细：

```js
const https = require('https');

const options = {
  hostname: 'example.com',
  port: 443,
  path: '/todos',
  method: 'GET',
};

const req = https.request(options, res => {
  console.log(`statusCode: ${res.statusCode}`);

  res.on('data', d => {
    process.stdout.write(d);
  });
});

req.on('error', error => {
  console.error(error);
});

req.end();
```

## 执行 POST 请求

与发出 HTTP GET 请求类似，您可以使用 [Axios 库](https://github.com/axios/axios) 库来执行 POST 请求：

```js
const axios = require('axios');

axios
  .post('https://whatever.com/todos', {
    todo: 'Buy the milk',
  })
  .then(res => {
    console.log(`statusCode: ${res.status}`);
    console.log(res);
  })
  .catch(error => {
    console.error(error);
  });

```

或者，使用 Node.js 标准模块：

```js
const https = require('https');

const data = JSON.stringify({
  todo: 'Buy the milk',
});

const options = {
  hostname: 'whatever.com',
  port: 443,
  path: '/todos',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Content-Length': data.length,
  },
};

const req = https.request(options, res => {
  console.log(`statusCode: ${res.statusCode}`);

  res.on('data', d => {
    process.stdout.write(d);
  });
});

req.on('error', error => {
  console.error(error);
});

req.write(data);
req.end();
```

## 放置和删除

PUT 和 DELETE 请求使用相同的 POST 请求格式 - 您只需将 `options.method` 值更改为适当的方法。
