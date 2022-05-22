# 使用 Node.js 读取文件

在 Node.js 中读取文件的最简单方法是使用 `fs.readFile()` 方法，将文件路径、编码和将与文件数据（和错误）一起调用的回调函数传递给它：

```js
const fs = require('fs');

fs.readFile('/Users/joe/test.txt', 'utf8', (err, data) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(data);
});
```

或者，您可以使用同步版本 `fs.readFileSync()`：

```js
const fs = require('fs');

try {
  const data = fs.readFileSync('/Users/joe/test.txt', 'utf8');
  console.log(data);
} catch (err) {
  console.error(err);
}
```

您还可以使用 `fs/promises` 模块提供的基于 promise 的 `fsPromises.readFile()` 方法：

```js
const fs = require('fs/promises');

async function example() {
  try {
    const data = await fs.readFile('/Users/joe/test.txt', { encoding: 'utf8' });
    console.log(data);
  } catch (err) {
    console.log(err);
  }
}
example();
```

`fs.readFile()`、`fs.readFileSync()` 和 `fsPromises.readFile()` 这三个都在返回数据之前读取内存中文件的全部内容。

这意味着大文件将对您的内存消耗和程序执行速度产生重大影响。

在这种情况下，更好的选择是使用流读取文件内容。
