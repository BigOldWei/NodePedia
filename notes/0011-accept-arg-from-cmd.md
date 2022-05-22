# Node.js从命令行接受参数

在调用 Node.js 应用程序时，您可以传递任意数量的参数

```bash
node app.js
```

参数可以是独立的，也可以具有键和值。

例如：

```bash
node app.js joe
```

或者

```bash
node app.js name=joe
```

这会改变您在 Node.js 代码中检索此值的方式。

检索它的方式是使用 Node.js 中内置的 `process` 对象。

它公开了一个 `argv` 属性，该属性是一个包含所有命令行调用参数的数组。

第一个元素是 `node` 命令的完整路径。

第二个元素是正在执行的文件的完整路径。

所有额外的论点都从第三个位置开始出现。

您可以使用循环遍历所有参数（包括Node路径和文件路径）：

```js
process.argv.forEach((val, index) => {
  console.log(`${index}: ${val}`);
});
```

您可以通过创建一个排除前 2 个参数的新数组来仅获取附加参数：

```js
const args = process.argv.slice(2);
```

如果您有一个没有索引名称的参数，如下所示：

```bash
node app.js joe
```

您可以使用它访问它

```js
const args = process.argv.slice(2);
args[0];
```

在这种情况下：

```bash
node app.js name=joe
```

`args[0]` 是 `name=joe`，你需要解析它。 最好的方法是使用 [`minimist`](https://www.npmjs.com/package/minimist) 库，它有助于处理参数：

```js
const args = require('minimist')(process.argv.slice(2));
args.name; // joe
```

使用 `npm` 安装所需的 `minimist` 包（关于包管理器的课程 [稍后](https://nodejs.dev/learn/an-introduction-to-the-npm-package-manager)）。

```bash
npm install minimist
```

这次您需要在每个参数名称之前使用双破折号：

```bash
node app.js --name=joe
```
