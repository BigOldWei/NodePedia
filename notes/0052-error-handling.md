# Node.js 中的错误处理

Node.js 中的错误是通过异常处理的。

## 创建异常

使用 `throw` 关键字创建异常：

```js
throw value;
```

一旦 JavaScript 执行了这一行，正常的程序流程就会停止，并且控制权会被保留到最近的**异常处理程序**。

通常在客户端代码中，`value` 可以是任何 JavaScript 值，包括字符串、数字或对象。

在 Node.js 中，我们不抛出字符串，我们只是抛出 Error 对象。

## 错误对象

错误对象是一个对象，它要么是 Error 对象的实例，要么扩展了 Error 类，在 [Error 核心模块](https://nodejs.org/api/errors.html) 中提供：

```js
throw new Error('Ran out of coffee');
```

或

```js
class NotEnoughCoffeeError extends Error {
  // ...
}
throw new NotEnoughCoffeeError();
```

## 处理异常

异常处理程序是一个 `try`/`catch` 语句。

在 `try` 块中包含的代码行中引发的任何异常都在相应的 `catch` 块中处理：

```js
try {
  // lines of code
} catch (e) {}
```

本例中的 `e` 是异常值。

您可以添加多个处理程序，它们可以捕获不同类型的错误。

## 捕获未捕获的异常

如果在程序执行期间抛出未捕获的异常，您的程序将崩溃。

为了解决这个问题，你在 `process` 对象上监听 `uncaughtException` 事件：

```js
process.on('uncaughtException', err => {
  console.error('There was an uncaught error', err);
  process.exit(1); // mandatory (as per the Node.js docs)
});
```

您不需要为此导入 `process` 核心模块，因为它是自动注入的。

## 带有承诺的异常

使用 Promise，您可以链接不同的操作，并在最后处理错误：

```js
doSomething1()
  .then(doSomething2)
  .then(doSomething3)
  .catch(err => console.error(err));
```

你怎么知道错误发生在哪里？ 你真的不知道，但是你可以在你调用的每个函数中处理错误（`doSomethingX`），并且在错误处理程序内部抛出一个新错误，它将调用外部的 `catch` 处理程序：

```js
const doSomething1 = () => {
  // ...
  try {
    // ...
  } catch (err) {
    // ... handle it locally
    throw new Error(err.message);
  }
  // ...
};
```

为了能够在本地处理错误而不在我们调用的函数中处理它们，我们可以打破链条。 您可以在每个 `then()` 中创建一个函数并处理异常：

```js
doSomething1()
  .then(() => {
    return doSomething2().catch(err => {
      // handle error
      throw err; // break the chain!
    });
  })
  .then(() => {
    return doSomething3().catch(err => {
      // handle error
      throw err; // break the chain!
    });
  })
  .catch(err => console.error(err));
```

## 使用 async/await 处理错误

使用 async/await，你仍然需要捕获错误，你可以这样做：

```js
async function someFunction() {
  try {
    await someOtherFunction();
  } catch (err) {
    console.error(err.message);
  }
}
```
