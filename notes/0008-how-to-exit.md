# 如何从node.js程序退出

有多种方法可以终止 Node.js 应用程序。

在控制台中运行程序时，您可以使用 ctrl-C 将其关闭，但我们在这里要讨论的是以编程方式退出。

让我们从最激烈的开始，看看为什么最好不要使用它。

process 核心模块提供了一个方便的方法，允许您以编程方式退出 Node.js 程序：process.exit()。

当 Node.js 运行这一行时，进程立即被强制终止。

这意味着任何待处理的回调、任何仍在发送的网络请求、任何文件系统访问或写入 stdout 或 stderr 的进程 - 都将立即被不正常地终止。

如果这对您来说没问题，您可以传递一个整数，向操作系统发出退出代码的信号：

```js
process.exit(1);
```

默认情况下，退出代码为 0，表示成功。 不同的退出代码具有不同的含义，您可能希望在自己的系统中使用它来让程序与其他程序通信。

您可以在 https://nodejs.org/api/process.html#process_exit_codes 阅读有关退出代码的更多信息

您还可以设置 process.exitCode 属性：

```js
process.exitCode = 1;
```

当程序结束时，Node.js 将返回该退出代码。

当所有处理完成时，程序将优雅地退出。

很多时候我们使用 Node.js 启动服务器，比如这个 HTTP 服务器：

```js
const express = require('express');
const app = express();
app.get('/', (req, res) => {  res.send('Hi!');});
app.listen(3000, () => console.log('Server ready'));
```

> Express 是一个在底层使用 http 模块的框架，app.listen() 返回一个 http 实例。 如果您需要使用 HTTPS 为您的应用程序提供服务，您将使用 https.createServer，因为 app.listen 仅使用 http 模块。

这个程序永远不会结束。 如果您调用 process.exit()，任何当前挂起或正在运行的请求都将被中止。 这不好。

最好在终止之前允许正在运行的请求完成。 在这种情况下，您需要向命令发送 SIGTERM 信号，并使用进程信号处理程序处理它：

> 注意：流程不需要“要求”，它是自动可用的。

```js
const express = require('express');
const app = express();
app.get('/', (req, res) => {  res.send('Hi!');});
const server = app.listen(3000, () => console.log('Server ready'));
process.on('SIGTERM', () => {  server.close(() => {    console.log('Process terminated');  });});
```

> 什么是信号？ 信号是 POSIX 互通系统：发送给进程的通知，以通知它发生的事件。

SIGKILL 是告诉进程立即终止的信号，理想情况下它的行为类似于 process.exit()。

SIGTERM 是告诉进程正常终止的信号。 它是从流程管理器（例如 upstart 或 supervisord 等）发出的信号。

您可以从程序内部的另一个函数中发送此信号：

```js
process.kill(process.pid, 'SIGTERM');
```


或者来自另一个运行 Node.js 的程序，或者在您的系统中运行的任何其他知道您要终止的进程的 PID 的应用程序。
