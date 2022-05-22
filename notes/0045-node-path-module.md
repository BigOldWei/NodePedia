# Node.js 路径模块
`path` 模块提供了许多非常有用的功能来访问和与文件系统交互。

无需安装它。 作为 Node.js 核心的一部分，只需要求它即可使用它：

```js
const path = require('path');
```

该模块提供了`path.sep`，它提供了路径段分隔符（在Windows上是`\`，在Linux / macOS上是`/`），以及提供路径分隔符的`path.delimiter`（在Windows上是`;`，和 `:` 在 Linux / macOS 上）。

这些是 `path` 方法：

### `path.basename()`

返回路径的最后一部分。 第二个参数可以过滤掉文件扩展名：

```js
require('path').basename('/test/something'); // something
require('path').basename('/test/something.txt'); // something.txt
require('path').basename('/test/something.txt', '.txt'); // something
```

### `path.dirname()`

返回路径的目录部分：

```js
require('path').dirname('/test/something'); // /test
require('path').dirname('/test/something/file.txt'); // /test/something
```

### `path.extname()`

返回路径的扩展部分

```js
require('path').extname('/test/something'); // ''
require('path').extname('/test/something/file.txt'); // '.txt'
```

### `path.format()`

从对象返回路径字符串，这与 `path.parse` 相反
`path.format` 接受一个对象作为具有以下键的参数：

- `root`：根
- `dir`：从根目录开始的文件夹路径
- `base`：文件名+扩展名
- `name`：文件名
- `ext`：文件扩展名

如果提供了 `dir`，则忽略 `root`
如果 `base` 存在，`ext` 和 `name` 将被忽略

```js
// POSIX
require('path').format({ dir: '/Users/joe', base: 'test.txt' }); //  '/Users/joe/test.txt'

require('path').format({ root: '/Users/joe', name: 'test', ext: '.txt' }); //  '/Users/joe/test.txt'

// WINDOWS
require('path').format({ dir: 'C:\\Users\\joe', base: 'test.txt' }); //  'C:\\Users\\joe\\test.txt'
```

### `path.isAbsolute()`

如果是绝对路径，则返回 true

```js
require('path').isAbsolute('/test/something'); // true
require('path').isAbsolute('./test/something'); // false
```

### `path.join()`

连接路径的两个或多个部分：

```js
const name = 'joe';
require('path').join('/', 'users', name, 'notes.txt'); // '/users/joe/notes.txt'
```

### `path.normalize()`

尝试计算实际路径时，它包含相对说明符，如 `.` 或 `..`，或双斜杠：

```js
require('path').normalize('/users/joe/..//test.txt'); // '/users/test.txt'
```

### `path.parse()`

用组成它的段解析一个对象的路径：

- `root`：根
- `dir`：从根目录开始的文件夹路径
- `base`：文件名+扩展名
- `name`：文件名
- `ext`：文件扩展名

例子：

```js
require('path').parse('/users/test.txt');
```

结果是

```console
{
  root: '/',
  dir: '/users',
  base: 'test.txt',
  ext: '.txt',
  name: 'test'
}
```

### `path.relative()`

接受 2 个路径作为参数。 根据当前工作目录返回从第一个路径到第二个路径的相对路径。

例子：

```js
require('path').relative('/Users/joe', '/Users/joe/test.txt'); // 'test.txt'
require('path').relative('/Users/joe', '/Users/joe/something/test.txt'); // 'something/test.txt'
```

### `path.resolve()`

您可以使用`path.resolve()`获取相对路径的绝对路径计算：

```js
require('path').resolve('joe.txt'); // '/Users/joe/joe.txt' if run from my home folder
```

通过指定第二个参数，`resolve` 将使用第一个作为第二个的基础：

```js
require('path').resolve('tmp', 'joe.txt'); // '/Users/joe/tmp/joe.txt' if run from my home folder
```

如果第一个参数以斜杠开头，则表示它是绝对路径：

```js
require('path').resolve('/etc', 'joe.txt'); // '/etc/joe.txt'
```

