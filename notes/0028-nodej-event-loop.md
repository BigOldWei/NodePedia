# Node.js 事件循环

## 介绍

**事件循环**是了解 Node.js 的最重要方面之一。

为什么这个这么重要？因为它解释了 Node.js 如何是异步的并且具有非阻塞 I/O，所以它基本上解释了 Node.js 的“杀手级特性”，它是让它如此成功的东西。

Node.js JavaScript 代码在单个线程上运行。一次只发生一件事。

这是一个实际上非常有用的限制，因为它大大简化了您的编程方式，而无需担心并发问题。

您只需要注意如何编写代码并避免任何可能阻塞线程的事情，例如同步网络调用或无限循环。

一般来说，在大多数浏览器中，每个浏览器选项卡都有一个事件循环，以使每个进程隔离，并避免网页无限循环或繁重的处理阻塞整个浏览器。

该环境管理多个并发事件循环，例如处理 API 调用。 Web Worker 也在它们自己的事件循环中运行。

您主要需要关注*您的代码* 将在单个事件循环上运行，并在编写代码时牢记这一点以避免阻塞它。

## 阻塞事件循环

任何需要很长时间才能将控制权返回给事件循环的 JavaScript 代码都会阻塞页面中任何 JavaScript 代码的执行，甚至阻塞 UI 线程，并且用户无法点击、滚动页面等等。

JavaScript 中几乎所有的 I/O 原语都是非阻塞的。网络请求、文件系统操作等。阻塞是个例外，这就是为什么 JavaScript 如此依赖回调，以及最近基于 Promise 和 async/await 的原因。

##调用栈

调用堆栈是 LIFO（后进先出）堆栈。

事件循环不断检查**调用堆栈**以查看是否有任何函数需要运行。

这样做时，它将找到的任何函数调用添加到调用堆栈并按顺序执行每个函数。

您知道调试器或浏览器控制台中您可能熟悉的错误堆栈跟踪吗？浏览器在调用堆栈中查找函数名称，以告知您哪个函数发起了当前调用：

[![Exception call stack](http://img.weidawang.site/i/2022/05/22/6289dfe860ca4.png)](https://nodejs.dev/static/e4594b6135efd353b44770f748fdccd5/1b853/exception-call-stack.png)

## A simple event loop explanation

让我们举个例子：

```javascript
const bar = () => console.log('bar')

const baz = () => console.log('baz')

const foo = () => {
  console.log('foo')
  bar()
  baz()
}

foo()
```

当这段代码运行时，首先调用 `foo()`。 在 `foo()` 中，我们首先调用 `bar()`，然后调用 `baz()`。

此时调用堆栈如下所示：

[![Call stack first example](http://img.weidawang.site/i/2022/05/22/6289dfe872956.png)](https://nodejs.dev/static/270ebeb6dbfa7d613152b71257c72a9e/11a8f/call-stack-first-example.png)

每次迭代的事件循环都会查看调用堆栈中是否有东西，并执行它：

[![Execution order first example](http://img.weidawang.site/i/2022/05/22/6289dfe864ffc.png)](https://nodejs.dev/static/ca404c319c6fc595497d5dc097d469ff/fc1a1/execution-order-first-example.png)

直到调用堆栈为空。

## 排队函数执行

上面的例子看起来很正常，并没有什么特别之处：JavaScript 找到要执行的东西，按顺序运行它们。

让我们看看如何推迟一个函数直到堆栈被清除。

`setTimeout(() => {}, 0)` 的用例是调用一个函数，但在代码中的每个其他函数执行后执行它。

举个例子：

```javascript
const bar = () => console.log('bar')

const baz = () => console.log('baz')

const foo = () => {
  console.log('foo')
  setTimeout(bar, 0)
  baz()
}

foo()
```

这段代码打印出来，也许令人惊讶：

```bash
foo
baz
bar
```

当此代码运行时，首先调用 foo()。 在 foo() 中，我们首先调用 setTimeout，将 `bar` 作为参数传递，然后我们指示它尽快运行，传递 0 作为计时器。 然后我们调用 baz()。

此时调用堆栈如下所示：

[![Call stack second example](http://img.weidawang.site/i/2022/05/22/6289dfe878500.png)](https://nodejs.dev/static/be55515b9343074d00b43de88c495331/966a0/call-stack-second-example.png)

以下是我们程序中所有函数的执行顺序：

[![Execution order second example](http://img.weidawang.site/i/2022/05/22/6289dfe87286c.png)](https://nodejs.dev/static/585ff3207d814911a7e44d55fbde483b/f96db/execution-order-second-example.png)

为什么会这样？

## 消息队列

调用 setTimeout() 时，浏览器或 Node.js 会启动计时器。一旦计时器到期，在这种情况下，我们立即将 0 作为超时，回调函数被放入**消息队列**中。

消息队列也是用户发起的事件（如单击或键盘事件）或获取响应在您的代码有机会对其做出反应之前排队的地方。或者像 `onload` 这样的 DOM 事件。

**循环优先考虑调用堆栈，它首先处理它在调用堆栈中找到的所有内容，一旦那里没有任何内容，它就会去获取消息队列中的内容。**

我们不必等待 `setTimeout`、fetch 或其他函数来完成自己的工作，因为它们是由浏览器提供的，并且它们存在于自己的线程中。例如，如果将 `setTimeout` 超时设置为 2 秒，则不必等待 2 秒 - 等待发生在其他地方。

## ES6 作业队列

ECMAScript 2015 引入了作业队列的概念，Promises 使用了它（在 ES6/ES2015 中也引入了）。这是一种尽快执行异步函数结果的方法，而不是放在调用堆栈的末尾。

在当前函数结束之前解析的 Promise 将在当前函数之后立即执行。

类似于游乐园的过山车：消息队列将您排在队列的最后，排在所有其他人之后，您必须在那里等待轮到您，而工作队列是让您乘坐的快速通行证完成上一个后立即进行另一次骑行。

例子：

```javascript
const bar = () => console.log('bar')

const baz = () => console.log('baz')

const foo = () => {
  console.log('foo')
  setTimeout(bar, 0)
  new Promise((resolve, reject) =>
    resolve('should be right after baz, before bar')
  ).then(resolve => console.log(resolve))
  baz()
}

foo()
```

这是 Promises（以及基于 Promise 构建的 Async/await）与通过 setTimeout() 或其他平台 API 实现的普通旧异步函数之间的巨大差异。

最后，这是上面示例的调用堆栈的样子：

[![Call stack third example](http://img.weidawang.site/i/2022/05/22/6289dfe876bc7.png)](https://nodejs.dev/static/4bb05f4d91852eb8c6b3de5371451315/f470a/call-stack-third-example.png)