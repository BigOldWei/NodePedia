# 在 Node.js 中使用文件描述符

在您能够与位于文件系统中的文件进行交互之前，您必须获得一个文件描述符。

文件描述符是对打开文件的引用，一个数字 (fd) 通过使用 `fs` 模块提供的 `open()` 方法打开文件返回。 此编号 (`fd`) 唯一标识操作系统中打开的文件：

```js
const fs = require('fs');

fs.open('/Users/joe/test.txt', 'r', (err, fd) => {
  // fd is our file descriptor
});
```

请注意我们用作 `fs.open()` 调用的第二个参数的 `r`。

该标志意味着我们打开文件进行读取。

您通常使用的其他标志是：

- `r+` 打开文件进行读写，如果文件不存在则不会创建。
- `w+` 打开文件进行读写，将流定位在文件的开头。 如果文件不存在，则创建该文件。
- `a` 打开文件进行写入，将流定位在文件末尾。 如果文件不存在，则创建该文件。
- `a+` 打开文件进行读写，将流定位在文件末尾。 如果文件不存在，则创建该文件。

您还可以使用 `fs.openSync` 方法打开文件，该方法返回文件描述符，而不是在回调中提供它：

```js
const fs = require('fs');

try {
  const fd = fs.openSync('/Users/joe/test.txt', 'r');
} catch (err) {
  console.error(err);
}
```

获得文件描述符后，无论选择何种方式，您都可以执行所有需要它的操作，例如调用 fs.close() 以及与文件系统交互的许多其他操作。

您还可以使用 `fs/promises` 模块提供的基于 promise 的 `fsPromises.open` 方法打开文件。

`fs/promises` 模块仅从 Node.js v14 开始可用。 在 v14 之前，v10 之后，您可以使用 `require('fs').promises` 代替。 在 v10 之前，v8 之后，您可以使用 `util.promisify` 将 `fs` 方法转换为基于 Promise 的方法。

```js
const fs = require('fs/promises');
// Or const fs = require('fs').promises before v14.
async function example() {
  let filehandle;
  try {
    filehandle = await fs.open('/Users/joe/test.txt', 'r');
    console.log(filehandle.fd);
    console.log(await filehandle.readFile({ encoding: 'utf8' }));
  } finally {
    await filehandle.close();
  }
}
example();
```

这是 util.promisify 的示例：

```js
const fs = require('fs');
const util = require('util');

async function example() {
  const open = util.promisify(fs.open);
  const fd = await open('/Users/joe/test.txt', 'r');
}
example();
```

要查看有关 `fs/promises` 模块的更多详细信息，请查看 [fs/promises API](https://nodejs.org/docs/latest-v17.x/api/fs.html#promises-api)。
