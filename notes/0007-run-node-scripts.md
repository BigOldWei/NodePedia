# 从命令行运行Node.js脚本

运行 Node.js 程序的常用方法是运行全局可用的 node 命令（安装 Node.js 后）并传递要执行的文件的名称。

如果您的主要 Node.js 应用程序文件是 app.js，您可以通过键入以下内容来调用它：

```bash
node app.js
```

上面，你明确告诉 shell 用 node.js 运行你的脚本。 您还可以使用“shebang”行将此信息嵌入到您的 JavaScript 文件中。 “shebang”是文件中的第一行，它告诉操作系统使用哪个解释器来运行脚本。 下面是 JavaScript 的第一行：

```js
#!/usr/bin/node
```

上面，我们明确给出了解释器的绝对路径。 并非所有操作系统的 bin 文件夹中都有Node，但都应该有 env。 您可以告诉操作系统以 node 作为参数运行 env：

```js
#!/usr/bin/env node
// your code
```

要使用 shebang，您的文件应该具有可执行权限。 您可以通过运行以下命令授予 app.js 可执行权限：

```bash
chmod u+x app.js
```

运行命令时，请确保您位于包含 app.js 文件的同一目录中。

## 自动重新启动应用程序

每当应用程序发生更改时，必须在 bash 中重新执行 node 命令，以自动重新启动应用程序，使用 nodemon 模块。

将 nodemon 模块全局安装到系统路径

```bash
npm i -g nodemon
```

您还可以将 nodemon 安装为开发依赖项

```bash
npm i --save-dev nodemon
```

可以通过从 npm 脚本（例如 npm start 或使用 npx nodemon）中调用它来运行 nodemon 的本地安装。

使用 nodemon 运行应用程序，后跟应用程序文件名。

```bash
nodemon app.js
```
