# 理解 setImmediate()

当您想异步执行一些代码，但要尽快执行时，一种选择是使用 Node.js 提供的 `setImmediate()` 函数：

```js
setImmediate(() => {
  // run something
});
```

任何作为 setImmediate() 参数传递的函数都是在事件循环的下一次迭代中执行的回调。

`setImmediate()` 与 `setTimeout(() => {}, 0)` （通过 0 毫秒超时）以及 `process.nextTick()` 和 `Promise.then()` 有何不同？

在当前操作结束后，传递给“process.nextTick()”的函数将在事件循环的当前迭代中执行。这意味着它将始终在 `setTimeout` 和 `setImmediate` 之前执行。

具有 0 毫秒延迟的 `setTimeout()` 回调与 `setImmediate()` 非常相似。执行顺序将取决于各种因素，但它们都将在事件循环的下一次迭代中运行。

一个 `process.nextTick` 回调被添加到 `process.nextTick 队列`。 `Promise.then()` 回调被添加到`promises microtask queue`。 `setTimeout`、`setImmediate` 回调被添加到`macrotask queue`。

事件循环首先执行`process.nextTick queue`中的任务，然后执行`promises microtask queue`，然后执行`macrotask queue`。

下面是一个示例，显示 `setImmediate()`、`process.nextTick()` 和 `Promise.then()` 之间的顺序：

```javascript
const baz = () => console.log('baz');
const foo = () => console.log('foo');
const zoo = () => console.log('zoo');
const start = () => {
  console.log('start');
  setImmediate(baz);
  new Promise((resolve, reject) => {
    resolve('bar');
  }).then((resolve) => {
    console.log(resolve);
    process.nextTick(zoo);
  });
  process.nextTick(foo);
};
start();

// start foo bar zoo baz
```

此代码将首先调用 `start()`，然后在 `process.nextTick queue` 中调用 `foo()`。 之后，它将处理`promises microtask queue`，打印`bar`并同时在`process.nextTick queue`中添加`zoo()`。 然后它会调用刚刚添加的`zoo()`。 最后，调用了`macrotask queue`中的`baz()`。

##### CONTRIBUTORS