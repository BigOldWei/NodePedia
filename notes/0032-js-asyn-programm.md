# JavaScript 异步编程和回调

## Asynchronicity in Programming Languages

计算机在设计上是异步的。

异步意味着事情可以独立于主程序流程发生。

在当前的消费类计算机中，每个程序都会运行一个特定的时间段，然后它会停止执行以让另一个程序继续执行。这东西循环运行得如此之快，以至于无法注意到。我们认为我们的计算机同时运行许多程序，但这是一种错觉（多处理器机器除外）。

程序内部使用 *interrupts*，这是一个发送到处理器以引起系统注意的信号。

现在让我们不要深入了解它的内部，但请记住，程序异步并暂停执行直到需要注意是正常的，同时允许计算机执行其他操作。当程序正在等待来自网络的响应时，它不能在请求完成之前停止处理器。

通常，编程语言是同步的，有些语言提供了一种管理语言或通过库的异步性的方法。默认情况下，C、Java、C#、PHP、Go、Ruby、Swift 和 Python 都是同步的。其中一些通过使用线程来处理异步操作，从而产生一个新进程。

## JavaScript

JavaScript 默认是**同步的**，并且是单线程的。这意味着代码无法创建新线程并并行运行。

代码行是一个接一个地依次执行的，例如：

```js
const a = 1;
const b = 2;
const c = a * b;
console.log(c);
doSomething();
```

但是 JavaScript 是在浏览器内部诞生的，它的主要工作，一开始是响应用户的操作，比如`onClick`、`onMouseOver`、`onChange`、`onSubmit`等等。 它如何使用同步编程模型做到这一点？

答案就在它的环境中。 **浏览器**通过提供一组可以处理此类功能的 API 来提供一种方法。

最近，Node.js 引入了非阻塞 I/O 环境，将这一概念扩展到文件访问、网络调用等。

## 回调

您无法知道用户何时会单击按钮。 因此，您**为单击事件定义一个事件处理程序**。 此事件处理程序接受一个函数，该函数将在事件触发时调用：

```js
document.getElementById('button').addEventListener('click', () => {
  // item clicked
});
```

这就是所谓的**回调**。

回调是一个简单的函数，它作为值传递给另一个函数，并且只会在事件发生时执行。 我们可以这样做是因为 JavaScript 具有一等函数，可以将其分配给变量并传递给其他函数（称为**高阶函数**）

将所有客户端代码包装在 `window` 对象上的 `load` 事件侦听器中是很常见的，它仅在页面准备好时运行回调函数：

```js
window.addEventListener('load', () => {
  // window loaded
  // do what you want
});
```

回调无处不在，不仅仅是在 DOM 事件中。

一个常见的例子是使用计时器：

```js
setTimeout(() => {
  // runs after 2 seconds
}, 2000);
```

XHR 请求也接受回调，在本例中，通过将函数分配给将在特定事件发生时调用的属性（在本例中，请求的状态发生变化）：

```js
const xhr = new XMLHttpRequest();
xhr.onreadystatechange = () => {
  if (xhr.readyState === 4) {
    xhr.status === 200 ? console.log(xhr.responseText) : console.error('error');
  }
};
xhr.open('GET', 'https://yoursite.com');
xhr.send();
```

## 处理回调中的错误

你如何处理回调错误？ 一个非常常见的策略是使用 Node.js 所采用的：任何回调函数中的第一个参数是错误对象：**error-first callbacks**

如果没有错误，则该对象为 `null`。 如果有错误，它包含一些错误描述和其他信息。

```js
const fs = require('fs');
fs.readFile('/file.json', (err, data) => {  if (err) {    // handle error    console.log(err);    return;  }
  // no errors, process data  console.log(data);});
```

## 回调的问题

回调非常适合简单的情况！

然而，每个回调都会增加一层嵌套，当你有很多回调时，代码很快就会变得复杂：

```js
window.addEventListener('load', () => {
  document.getElementById('button').addEventListener('click', () => {
    setTimeout(() => {
      items.forEach(item => {
        // your code here
      });
    }, 2000);
  });
});
```

这只是一个简单的 4 级代码，但我见过更多级别的嵌套，这并不有趣。

我们如何解决这个问题？

## 回调的替代方案

从 ES6 开始，JavaScript 引入了一些特性来帮助我们编写不涉及使用回调的异步代码：Promises (ES6) 和 Async/Await (ES2017)。
