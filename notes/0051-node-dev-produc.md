# Node.js，开发和生产的区别

您可以为生产和开发环境提供不同的配置。

Node.js 假定它始终在开发环境中运行。 您可以通过设置 `NODE_ENV=production` 环境变量来向 Node.js 发出您正在生产中运行的信号。

这通常通过执行命令来完成

```bash
export NODE_ENV=production
```

在 shell 中，但最好将它放在你的 shell 配置文件中（例如，使用 Bash shell 的 `.bash_profile`），因为否则在系统重新启动的情况下设置不会持续存在。

您还可以通过将环境变量附加到应用程序初始化命令来应用环境变量：

```bash
NODE_ENV=production node app.js
```

此环境变量是一种在外部库中也广泛使用的约定。

将环境设置为“生产”通常可以确保

- 日志记录保持在最低限度的基本水平
- 发生更多缓存级别以优化性能

例如 Pug，Express 使用的模板库，如果 `NODE_ENV` 未设置为 `production`，则在调试模式下编译。 Express 视图在开发模式下的每个请求中都被编译，而在生产中它们被缓存。 还有很多例子。

您可以使用条件语句在不同的环境中执行代码：

```js
if (process.env.NODE_ENV === 'development') {
  // ...
}
if (process.env.NODE_ENV === 'production') {
  // ...
}
if (['production', 'staging'].indexOf(process.env.NODE_ENV) >= 0) {
  // ...
}
```

例如，在 Express 应用程序中，您可以使用它为每个环境设置不同的错误处理程序：

```js
if (process.env.NODE_ENV === 'development') {
  app.use(express.errorHandler({ dumpExceptions: true, showStack: true }));
}

if (process.env.NODE_ENV === 'production') {
  app.use(express.errorHandler());
}
```
