# 发现 JavaScript 计时器

## `setTimeout()`

在编写 JavaScript 代码时，您可能希望延迟函数的执行。

这是`setTimeout`的工作。 您指定一个稍后执行的回调函数，以及一个表示您希望它在多长时间后运行的值，以毫秒为单位：

```js
JScopysetTimeout(() => {  // runs after 2 seconds}, 2000);
setTimeout(() => {  // runs after 50 milliseconds}, 50);
```

此语法定义了一个新函数。 你可以在那里调用任何你想要的函数，或者你可以传递一个现有的函数名和一组参数：

```js
JScopyconst myFunction = (firstParam, secondParam) => {  // do something};
// runs after 2 secondssetTimeout(myFunction, 2000, firstParam, secondParam);
```

`setTimeout` 返回计时器 ID。 这个一般不用，但是可以把这个id存起来，如果要删除这个预定的函数执行，可以清空：

```js
JScopyconst id = setTimeout(() => {  // should run after 2 seconds}, 2000);
// I changed my mindclearTimeout(id);
```

### Zero delay

如果将超时延迟指定为`0`，则回调函数将尽快执行，但在当前函数执行之后：

```js
setTimeout(() => {  console.log('after ');}, 0);
console.log(' before ');
```

此代码将打印

```bash
before
after
```

通过在调度程序中对函数进行排队，这对于避免在密集任务上阻塞 CPU 并在执行繁重计算时执行其他函数特别有用。

> 一些浏览器（IE 和 Edge）实现了一个 `setImmediate()` 方法来执行相同的功能，但它不是标准的并且[在其他浏览器上不可用](https://caniuse.com/#feat=setimmediate)。 但它是 Node.js 中的标准函数。

## `setInterval()`

`setInterval` 是一个类似于 `setTimeout` 的函数，不同之处在于：它不会运行一次回调函数，而是会永远运行它，在您指定的特定时间间隔（以毫秒为单位）：

```js
setInterval(() => {
  // runs every 2 seconds
}, 2000);
```

上面的函数每 2 秒运行一次，除非你告诉它停止，使用 `clearInterval`，将 `setInterval` 返回的间隔 id 传递给它：

```js
const id = setInterval(() => {  // runs every 2 seconds}, 2000);
clearInterval(id);
```

在 setInterval 回调函数中调用 `clearInterval` 是很常见的，让它自动确定是否应该再次运行或停止。 例如，除非 App.somethingIWait 的值为 `arrived`，否则此代码会运行某些内容：

```js
const interval = setInterval(() => {
  if (App.somethingIWait === 'arrived') {
    clearInterval(interval);
  }
  // otherwise do things
}, 100);
```

## Recursive setTimeout

`setInterval` 每 n 毫秒启动一个函数，不考虑函数何时完成执行。

如果一个函数总是花费相同的时间，这一切都很好：

[![setInterval working fine](http://img.weidawang.site/i/2022/05/22/6289e1d28f60c.png)](https://nodejs.dev/static/fa9e9fec1aea517d98b47b11c5fec296/4d383/setinterval-ok.png)

也许该函数需要不同的执行时间，具体取决于网络条件，例如：

[![setInterval varying duration](http://img.weidawang.site/i/2022/05/22/6289e1d3175ca.png)](https://nodejs.dev/static/f2ae544ad5038515ba1d44b29322bec9/19a6b/setinterval-varying-duration.png)

也许一个长时间的执行与下一个重叠：

[![setInterval overlapping](http://img.weidawang.site/i/2022/05/22/6289e1d314f4b.png)](https://nodejs.dev/static/4e64c07dfb9f7be0e819fe3eb7def66a/393aa/setinterval-overlapping.png)

为避免这种情况，您可以安排在回调函数完成时调用递归 setTimeout：

```js
const myFunction = () => {  // do something
  setTimeout(myFunction, 1000);};
setTimeout(myFunction, 1000);
```

实现这个场景：

[![Recursive setTimeout](http://img.weidawang.site/i/2022/05/22/6289e1d2d64a9.png)](https://nodejs.dev/static/4bde07363650160e953f899734adc29e/1790f/recursive-settimeout.png)

`setTimeout` 和 `setInterval` 在 Node.js 中通过 [Timers 模块](https://nodejs.org/api/timers.html) 可用。

Node.js 还提供了 `setImmediate()`，相当于使用了 `setTimeout(() => {}, 0)`，主要用于与 Node.js 事件循环配合使用。