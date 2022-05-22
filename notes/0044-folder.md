# 在 Node.js 中使用文件夹

Node.js `fs` 核心模块提供了许多方便的方法来处理文件夹。

## 检查文件夹是否存在

使用 `fs.access()`（及其基于 promise 的 `fsPromises.access()` 对应项）检查文件夹是否存在并且 Node.js 可以使用其权限访问它。

＃＃ 新建一个文件夹

使用 `fs.mkdir()` 或 `fs.mkdirSync()` 或 `fsPromises.mkdir()` 创建一个新文件夹。

```js
const fs = require('fs');

const folderName = '/Users/joe/test';

try {
  if (!fs.existsSync(folderName)) {
    fs.mkdirSync(folderName);
  }
} catch (err) {
  console.error(err);
}
```

## 读取目录的内容

使用 `fs.readdir()` 或 `fs.readdirSync()` 或 `fsPromises.readdir()` 来读取目录的内容。

这段代码读取文件夹的内容，包括文件和子文件夹，并返回它们的相对路径：

```js
const fs = require('fs');
const folderPath = '/Users/joe';
fs.readdirSync(folderPath);
```

您可以获得完整路径：

```js
fs.readdirSync(folderPath).map(fileName => {
  return path.join(folderPath, fileName);
});
```

您还可以过滤结果以仅返回文件，并排除文件夹：

```js
const isFile = fileName => {
  return fs.lstatSync(fileName).isFile();
};

fs.readdirSync(folderPath)
  .map(fileName => {
    return path.join(folderPath, fileName);
  })
  .filter(isFile);
```

## 重命名文件夹

使用 `fs.rename()` 或 `fs.renameSync()` 或 `fsPromises.rename()` 重命名文件夹。 第一个参数是当前路径，第二个是新路径：

```js
const fs = require('fs');

fs.rename('/Users/joe', '/Users/roger', err => {
  if (err) {
    console.error(err);
  }
  // done
});
```

`fs.renameSync()` 是同步版本：

```js
const fs = require('fs');

try {
  fs.renameSync('/Users/joe', '/Users/roger');
} catch (err) {
  console.error(err);
}
```

`fsPromises.rename()` 是基于 promise 的版本：

```js
const fs = require('fs/promises');

async function example() {
  try {
    await fs.rename('/Users/joe', '/Users/roger');
  } catch (err) {
    console.log(err);
  }
}
example();
```

## 删除一个文件夹

使用 `fs.rmdir()` 或 `fs.rmdirSync()` 或 `fsPromises.rmdir()` 删除文件夹。

删除包含内容的文件夹可能比您需要的更复杂。 您可以通过选项 `{ recursive: true }` 以递归方式删除内容。

```js
const fs = require('fs');

fs.rmdir(dir, { recursive: true }, err => {
  if (err) {
    throw err;
  }

  console.log(`${dir} is deleted!`);
});
```

> **注意：** 在 Node `v16.x` 中，对于回调 API 的 `fs.rmdir`，选项 `recursive` 已被**弃用**，而是使用 `fs.rm` 删除其中包含内容的文件夹：

```js
const fs = require('fs');

fs.rm(dir, { recursive: true, force: true }, err => {
  if (err) {
    throw err;
  }

  console.log(`${dir} is deleted!`);
});
```

或者您可以安装并使用非常流行且维护良好的 [`fs-extra`](https://www.npmjs.com/package/fs-extra) 模块。 它是 `fs` 模块的直接替代品，在其之上提供了更多功能。

在这种情况下，`remove()` 方法就是你想要的。

使用安装它

```bash
npm install fs-extra
```

并像这样使用它：

```js
const fs = require('fs-extra');

const folder = '/Users/joe';

fs.remove(folder, err => {
  console.error(err);
});
```

它也可以与 Promise 一起使用：

```js
fs.remove(folder)
  .then(() => {
    // done
  })
  .catch(err => {
    console.error(err);
  });
```

或使用异步/等待：

```js
async function removeFolder(folder) {
  try {
    await fs.remove(folder);
    // done
  } catch (err) {
    console.error(err);
  }
}

const folder = '/Users/joe';
removeFolder(folder);
```

