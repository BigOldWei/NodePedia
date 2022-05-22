# 了解 JavaScript Promise

## Promises简介

```javascript
let done = true

const isItDoneYet = new Promise((resolve, reject) => {
  if (done) {
    const workDone = 'Here is the thing I built'
    resolve(workDone)
  } else {
    const why = 'Still working on something else'
    reject(why)
  }
})

const checkIfItsDone = () => {
  isItDoneYet
    .then(ok => {
      console.log(ok)
    })
    .catch(err => {
      console.error(err)
    })
}

checkIfItsDone()
```

承诺通常被定义为**最终将可用的值的代理**。

Promise 是处理异步代码的一种方法，不会陷入 [回调地狱](http://callbackhell.com/)。

Promise 多年来一直是该语言的一部分（在 ES2015 中标准化和引入），并且最近变得更加集成，在 ES2017 中具有 **async** 和 **await**。

**异步函数**在幕后使用 Promise，因此了解 Promise 的工作原理对于理解 `async` 和 `await` 的工作方式至关重要。

### 简而言之，promise 是如何工作的

一旦一个 promise 被调用，它将以 **pending 状态**开始。这意味着调用函数会继续执行，而 Promise 在它解决之前一直处于挂起状态，从而为调用函数提供所请求的任何数据。

创建的 Promise 最终会以 **resolved state** 或 **rejected state** 结束，完成后调用各自的回调函数（传递给 `then` 和 `catch`）。

### 哪些 JS API 使用 Promise？

除了您自己的代码和库代码之外，标准现代 Web API 还使用 Promise，例如：

- 电池 API
- 获取 API
- 服务人员

在现代 JavaScript 中，您不太可能发现自己*不*使用 Promise，所以让我们开始深入研究它们。

------

## 创建一个承诺

Promise API 公开了一个 Promise 构造函数，您可以使用 `new Promise()` 对其进行初始化：

```js
const done = true;

const isItDoneYet = new Promise((resolve, reject) => {
  if (done) {
    const workDone = 'Here is the thing I built';
    resolve(workDone);
  } else {
    const why = 'Still working on something else';
    reject(why);
  }
});

```

如您所见，promise 检查 `done` 全局常量，如果为真，则 promise 进入 **resolved** 状态（因为调用了 `resolve` 回调）；否则，将执行 `reject` 回调，将 Promise 置于拒绝状态。 （如果在执行路径中没有调用这些函数，promise 将保持挂起状态）

使用 `resolve` 和 `reject`，我们可以向调用者传达最终的承诺状态是什么，以及如何处理它。在上面的例子中，我们只是返回了一个字符串，但它可以是一个对象，也可以是 `null`。因为我们在上面的代码片段中创建了 Promise，所以它**已经开始执行**。这对于理解下面的 [Consuming a promise](https://nodejs.dev/learn/understanding-javascript-promises#sumption-a-promise) 部分的内容很重要。

您可能遇到的一个更常见的示例是一种称为 **Promisifying** 的技术。这种技术是一种能够使用带有回调的经典 JavaScript 函数并让它返回 Promise 的方法：

```js
const fs = require('fs');

const getFile = fileName => {
  return new Promise((resolve, reject) => {
    fs.readFile(fileName, (err, data) => {
      if (err) {
        reject(err); // calling `reject` will cause the promise to fail with or without the error passed as an argument
        return; // and we don't want to go any further
      }
      resolve(data);
    });
  });
};

getFile('/etc/passwd')
  .then(data => console.log(data))
  .catch(err => console.error(err));

```

> 在 Node.js 的最新版本中，您不必为很多 API 进行这种手动转换。 [util 模块](https://nodejs.org/docs/latest-v11.x/api/util.html#util_util_promisify_original) 中有一个有前途的功能可以为您执行此操作，前提是您的功能' re promisifying 具有正确的签名。

------

## 使用承诺

在上一节中，我们介绍了如何创建 Promise。

现在，让我们看看如何*消费*或使用承诺。

```js
const isItDoneYet = new Promise(/* ... as above ... */);
// ...

const checkIfItsDone = () => {
  isItDoneYet
    .then(ok => {
      console.log(ok);
    })
    .catch(err => {
      console.error(err);
    });
};
```

运行 `checkIfItsDone()` 将指定在 `isItDoneYet` 承诺解决（在 `then` 调用中）或拒绝（在 `catch` 调用中）时执行的函数。

------

## 链接承诺

一个 Promise 可以返回到另一个 Promise，创建一个 Promise 链。

链式 Promise 的一个很好的例子是 Fetch API，我们可以使用它来获取资源并在获取资源时排队执行一系列 Promise。

Fetch API 是基于 Promise 的机制，调用 fetch() 相当于使用 new Promise() 定义我们自己的 Promise。

### 链接承诺的示例

```js
const status = response => {
  if (response.status >= 200 && response.status < 300) {
    return Promise.resolve(response);
  }
  return Promise.reject(new Error(response.statusText));
};

const json = response => response.json();

fetch('/todos.json')
  .then(status) // note that the `status` function is actually **called** here, and that it **returns a promise***
  .then(json) // likewise, the only difference here is that the `json` function here returns a promise that resolves with `data`
  .then(data => {
    // ... which is why `data` shows up here as the first parameter to the anonymous function
    console.log('Request succeeded with JSON response', data);
  })
  .catch(error => {
    console.log('Request failed', error);
  });
```

> [`node-fetch`](https://www.npmjs.com/package/node-fetch) 是 Node.js 运行时上 window.fetch 兼容 API 的最少代码。

在这个例子中，我们调用 fetch() 来从域根目录中的 todos.json 文件中获取 TODO 项的列表，并且我们创建了一个 Promise 链。

运行 `fetch()` 会返回一个 [response](https://fetch.spec.whatwg.org/#concept-response)，它有很多属性，在我们引用的这些属性中：

- `status`，一个表示 HTTP 状态码的数值
- `statusText`，状态信息，如果请求成功则为`OK`

`response` 还有一个 `json()` 方法，它返回一个 promise，该 promise 将处理正文内容并转换为 JSON。

因此，鉴于这些承诺，会发生以下情况：链中的第一个承诺是我们定义的一个函数，称为“status()”，它检查响应状态，如果它不是成功响应（介于 200 和 299 之间），它拒绝承诺。

此操作将导致 Promise 链跳过列出的所有链式 Promise，并直接跳到底部的 `catch()` 语句，记录 `Request failed` 文本以及错误消息。

如果成功，它会调用我们定义的 `json()` 函数。由于前一个 Promise 在成功时返回了 `response` 对象，我们将其作为第二个 Promise 的输入。

在这种情况下，我们返回处理后的数据 JSON，因此第三个 Promise 直接接收 JSON：

```js
fetch('/todos.json')
  .then(status)
  .then(json)
  .then(data => {
    console.log('Request succeeded with JSON response', data);
  });
```

我们只需将其记录到控制台。

------

## 处理错误

在上一节的示例中，我们有一个附加到 Promise 链的“catch”。

当 Promise 链中的任何内容失败并引发错误或拒绝 Promise 时，控件将转到链下最近的“catch()”语句。

```js
new Promise((resolve, reject) => {  throw new Error('Error');}).catch(err => {  console.error(err);});
// or
new Promise((resolve, reject) => {  reject('Error');}).catch(err => {  console.error(err);});
```

### 级联错误

如果在 `catch()` 中引发错误，您可以附加第二个 `catch()` 来处理它，依此类推。

```js
new Promise((resolve, reject) => {
  throw new Error('Error');
})
  .catch(err => {
    throw new Error('Error');
  })
  .catch(err => {
    console.error(err);
  });
```

------

## 编排承诺

### `Promise.all()`

如果您需要同步不同的 Promise，`Promise.all()` 可以帮助您定义一个 Promise 列表，并在它们全部解决后执行某些操作。

例子：

```js
const f1 = fetch('/something.json');const f2 = fetch('/something2.json');
Promise.all([f1, f2])  .then(res => {    console.log('Array of results', res);  })  .catch(err => {    console.error(err);  });
```

ES2015 解构赋值语法允许你也做

```js
Promise.all([f1, f2]).then(([res1, res2]) => {
  console.log('Results', res1, res2);
});
```

当然，您不仅限于使用 `fetch`，**任何 promise 都可以以这种方式使用**。

### `Promise.race()`

`Promise.race()` 在你传递给它的第一个 Promise 解决（解决或拒绝）时运行，并且它只运行一次附加的回调，第一个 Promise 的结果已经解决。

例子：

```js
const first = new Promise((resolve, reject) => {  setTimeout(resolve, 500, 'first');});const second = new Promise((resolve, reject) => {  setTimeout(resolve, 100, 'second');});
Promise.race([first, second]).then(result => {  console.log(result); // second});
```

### `Promise.any()`

`Promise.any()` 当你传递给它的任何承诺履行或所有承诺被拒绝时解决。 它返回一个单一的 Promise，该 Promise 使用第一个已实现的 Promise 中的值进行解析。 如果所有的 Promise 都被拒绝，那么返回的 Promise 会被一个 `AggregateError` 拒绝。

例子：

```js
const first = new Promise((resolve, reject) => {  setTimeout(reject, 500, 'first');});const second = new Promise((resolve, reject) => {  setTimeout(reject, 100, 'second');});
Promise.any([first, second]).catch(error => {  console.log(error); // AggregateError});
```

## 常见错误

### Uncaught TypeError: undefined is not a promise

如果您在控制台中收到 `Uncaught TypeError: undefined is not a promise` 错误，请确保使用 `new Promise()` 而不仅仅是 `Promise()`

### UnhandledPromiseRejectionWarning

这意味着您调用的 promise 被拒绝，但没有用于处理错误的`catch`。 在有问题的 `then` 之后添加一个 `catch` 以正确处理此问题。
