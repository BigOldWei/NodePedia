# Node.js 文件统计信息

每个文件都带有一组细节，我们可以使用 Node.js 进行检查。

特别是，使用 `fs` 模块提供的 `stat()` 方法。

您调用它传递文件路径，一旦 Node.js 获取文件详细信息，它将调用您传递的回调函数，带有 2 个参数：错误消息和文件统计信息：

```js
const fs = require('fs');

fs.stat('/Users/joe/test.txt', (err, stats) => {
  if (err) {
    console.error(err);
  }
  // we have access to the file stats in `stats`
});
```

Node.js 还提供了一个同步方法，它会阻塞线程，直到文件统计信息准备好：

```js
const fs = require('fs');

try {
  const stats = fs.statSync('/Users/joe/test.txt');
} catch (err) {
  console.error(err);
}
```

文件信息包含在 stats 变量中。 我们可以使用统计数据提取什么样的信息？

很多，包括：

- 如果文件是目录或文件，使用 `stats.isFile()` 和 `stats.isDirectory()`
- 如果文件是使用 `stats.isSymbolicLink()` 的符号链接
- 使用 `stats.size` 的文件大小（以字节为单位）。

还有其他高级方法，但您将在日常编程中使用的大部分内容就是这些。

```js
const fs = require('fs');

fs.stat('/Users/joe/test.txt', (err, stats) => {
  if (err) {
    console.error(err);
    return;
  }

  stats.isFile(); // true
  stats.isDirectory(); // false
  stats.isSymbolicLink(); // false
  stats.size; // 1024000 //= 1MB
});
```

如果您愿意，还可以使用 `fs/promises` 模块提供的基于 promise 的 `fsPromises.stat()` 方法：

```js
const fs = require('fs/promises');

async function example() {
  try {
    const stats = await fs.stat('/Users/joe/test.txt');
    stats.isFile(); // true
    stats.isDirectory(); // false
    stats.isSymbolicLink(); // false
    stats.size; // 1024000 //= 1MB
  } catch (err) {
    console.log(err);
  }
}
example();
```

