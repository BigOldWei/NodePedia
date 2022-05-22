# 如何从 Node.js 读取环境变量

Node.js 的进程核心模块提供了 env 属性，该属性托管了在进程启动时设置的所有环境变量。

下面的代码运行 app.js 并设置 USER_ID 和 USER_KEY。

```bash
USER_ID=239482 USER_KEY=foobar node app.js
```

这会将用户 USER_ID 作为 239482 并将 USER_KEY 作为 foobar 传递。 这适用于测试，但是对于生产，您可能需要配置一些 bash 脚本来导出变量。

> Note: `process` does not require a "require", it's automatically available.

这是一个访问我们在上面代码中设置的 `USER_ID` 和 `USER_KEY` 环境变量的示例。

```js
process.env.USER_ID; // "239482"
process.env.USER_KEY; // "foobar"
```

以同样的方式，您可以访问您设置的任何自定义环境变量。

如果你的node项目中有多个环境变量，你也可以在你的项目根目录下创建一个`.env`文件，然后使用[dotenv](https://www.npmjs.com/package/dotenv ) 包以在运行时加载它们。

```bash
# .env file
USER_ID="239482"
USER_KEY="foobar"
NODE_ENV="development"
```

在你的 js 文件中

```js
require('dotenv').config();

process.env.USER_ID; // "239482"
process.env.USER_KEY; // "foobar"
process.env.NODE_ENV; // "development"
```

> 如果您不想在代码中导入包，也可以使用 `node -r dotenv/config index.js` 命令运行 js 文件。