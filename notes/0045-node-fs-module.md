# Node.js fs 模块

`fs` 模块提供了许多非常有用的功能来访问和与文件系统交互。

无需安装它。 作为 Node.js 核心的一部分，只需要求它即可使用它：

```js
const fs = require('fs');
```

一旦你这样做，你就可以访问它的所有方法，其中包括：

- `fs.access()`: 检查文件是否存在以及 Node.js 是否可以使用它的权限访问它
- `fs.appendFile()`：将数据附加到文件中。如果文件不存在，则创建
- `fs.chmod()`：更改由传递的文件名指定的文件的权限。相关：`fs.lchmod()`、`fs.fchmod()`
- `fs.chown()`：更改由传递的文件名指定的文件的所有者和组。相关：`fs.fchown()`、`fs.lchown()`
- `fs.close()`: 关闭文件描述符
- `fs.copyFile()`：复制文件
- `fs.createReadStream()`: 创建一个可读的文件流
- `fs.createWriteStream()`: 创建一个可写的文件流
- `fs.link()`: 创建一个新的文件硬链接
- `fs.mkdir()`: 创建一个新文件夹
- `fs.mkdtemp()`: 创建一个临时目录
- `fs.open()`：打开文件并返回文件描述符以允许文件操作
- `fs.readdir()`：读取目录的内容
- `fs.readFile()`：读取文件的内容。相关：`fs.read()`
- `fs.readlink()`：读取符号链接的值
- `fs.realpath()`：将相对文件路径指针（`.`、`..`）解析为完整路径
- `fs.rename()`：重命名文件或文件夹
- `fs.rmdir()`：删除文件夹
- `fs.stat()`：返回由传递的文件名标识的文件的状态。相关：`fs.fstat()`、`fs.lstat()`
- `fs.symlink()`: 创建一个指向文件的新符号链接
- `fs.truncate()`：将传递的文件名标识的文件截断到指定长度。相关：`fs.ftruncate()`
- `fs.unlink()`：删除文件或符号链接
-`fs.unwatchFile()`：停止监视文件的更改
- `fs.utimes()`：更改由传递的文件名标识的文件的时间戳。相关：`fs.futimes()`
- `fs.watchFile()`：开始观察文件的变化。相关：`fs.watch()`
- `fs.writeFile()`：将数据写入文件。相关：`fs.write()`

`fs` 模块的一个特殊之处在于所有方法默认都是异步的，但它们也可以通过附加 `Sync` 来同步工作。

例如：

- `fs.rename()`
- `fs.renameSync()`
- `fs.write()`
- `fs.writeSync()`

这会对您的应用程序流程产生巨大影响。

> Node.js 10 包含 [实验性支持](https://nodejs.org/api/fs.html#fs_fs_promises_api)，用于基于 Promise 的 API

例如，让我们检查 `fs.rename()` 方法。异步 API 与回调一起使用：

```js
const fs = require('fs');

fs.rename('before.json', 'after.json', err => {
  if (err) {
    return console.error(err);
  }

  // done
});
```

可以像这样使用同步 API，使用 try/catch 块来处理错误：

```js
const fs = require('fs');

try {
  fs.renameSync('before.json', 'after.json');
  // done
} catch (err) {
  console.error(err);
}
```

这里的关键区别在于，在第二个示例中，脚本的执行将阻塞，直到文件操作成功。

您可以使用 `fs/promises` 模块提供的基于 promise 的 API 来避免使用基于回调的 API，这可能会导致 [回调地狱](http://callbackhell.com/)。 这是一个例子：

```js
// Example: Read a file and change its content and read
// it again using callback-based API.
const fs = require('fs');

const fileName = '/Users/joe/test.txt';
fs.readFile(fileName, 'utf8', (err, data) => {
  if (err) {
    console.log(err);
    return;
  }
  console.log(data);
  const content = 'Some content!';
  fs.writeFile(fileName, content, err2 => {
    if (err2) {
      console.log(err2);
      return;
    }
    console.log('Wrote some content!');
    fs.readFile(fileName, 'utf8', (err3, data3) => {
      if (err3) {
        console.log(err3);
        return;
      }
      console.log(data3);
    });
  });
});
```

当嵌套回调太多时，基于回调的 API 可能会引发回调地狱。 我们可以简单地使用基于 Promise 的 API 来避免它：

```js
// Example: Read a file and change its content and read
// it again using promise-based API.
const fs = require('fs/promises');

async function example() {
  const fileName = '/Users/joe/test.txt';
  try {
    const data = await fs.readFile(fileName, 'utf8');
    console.log(data);
    const content = 'Some content!';
    await fs.writeFile(fileName, content);
    console.log('Wrote some content!');
    const newData = await fs.readFile(fileName, 'utf8');
    console.log(newData);
  } catch (err) {
    console.log(err);
  }
}
example();
```
