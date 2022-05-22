# 使用 Node.js 编写文件

在 Node.js 中写入文件的最简单方法是使用 `fs.writeFile()` API。

例子：

```js
const fs = require('fs');

const content = 'Some content!';

fs.writeFile('/Users/joe/test.txt', content, err => {
  if (err) {
    console.error(err);
  }
  // file written successfully
});
```

或者，您可以使用同步版本 `fs.writeFileSync()`：

```js
const fs = require('fs');

const content = 'Some content!';

try {
  fs.writeFileSync('/Users/joe/test.txt', content);
  // file written successfully
} catch (err) {
  console.error(err);
}
```

您还可以使用 `fs/promises` 模块提供的基于 promise 的 `fsPromises.writeFile()` 方法：

```js
const fs = require('fs/promises');

async function example() {
  try {
    const content = 'Some content!';
    await fs.writeFile('/Users/joe/test.txt', content);
  } catch (err) {
    console.log(err);
  }
}
example();
```

默认情况下，如果文件已存在，此 API 将**替换文件的内容**。

您可以通过指定标志来修改默认值：

```js
fs.writeFile('/Users/joe/test.txt', content, { flag: 'a+' }, err => {});
```

您可能会使用的标志是

- `r+` 打开文件进行读写
- `w+` 打开文件进行读写，将流定位在文件的开头。 如果文件不存在，则创建该文件
- `a` 打开文件进行写入，将流定位在文件末尾。 如果文件不存在，则创建该文件
- `a+` 打开文件进行读写，将流定位在文件末尾。 如果文件不存在，则创建该文件

（您可以在 https://nodejs.org/api/fs.html#fs_file_system_flags 找到更多标志）

## 附加到文件

将内容附加到文件末尾的一种方便方法是`fs.appendFile()`（及其对应的`fs.appendFileSync()`）：

```js
const content = 'Some content!';

fs.appendFile('file.log', content, err => {
  if (err) {
    console.error(err);
  }
  // done!
});
```

这是一个`fsPromises.appendFile()`的例子：

```js
const fs = require('fs/promises');

async function example() {
  try {
    const content = 'Some content!';
    await fs.appendFile('/Users/joe/test.txt', content);
  } catch (err) {
    console.log(err);
  }
}
example();
```

## 使用流

所有这些方法在将控制权返回给您的程序之前将全部内容写入文件（在异步版本中，这意味着执行回调）

在这种情况下，更好的选择是使用流写入文件内容。