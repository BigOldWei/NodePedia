# Node.js 文件路径

系统中的每个文件都有一个路径。

在 Linux 和 macOS 上，路径可能如下所示：

```
/users/joe/file.txt
```

而 Windows 计算机则不同，其结构如下：

```
C:\users\joe\file.txt
```

在应用程序中使用路径时需要注意，因为必须考虑到这种差异。

你在你的文件中包含这个模块，使用

```js
const path = require('path');
```

你可以开始使用它的方法了。

## 从路径中获取信息

给定路径，您可以使用以下方法从中提取信息：

- `dirname`: 获取文件的父文件夹
- `basename`：获取文件名部分
- `extname`：获取文件扩展名

例子：

```js
const notes = '/users/joe/notes.txt';

path.dirname(notes); // /users/joe
path.basename(notes); // notes.txt
path.extname(notes); // .txt
```

您可以通过为 `basename` 指定第二个参数来获取不带扩展名的文件名：

```js
path.basename(notes, path.extname(notes)); // notes
```

## 使用路径

您可以使用 `path.join()` 连接路径的两个或多个部分：

```js
const name = 'joe';
path.join('/', 'users', name, 'notes.txt'); // '/users/joe/notes.txt'
```

您可以使用`path.resolve()`获取相对路径的绝对路径计算：

```js
path.resolve('joe.txt'); // '/Users/joe/joe.txt' if run from my home folder
```

在这种情况下，Node.js 将简单地将 `/joe.txt` 附加到当前工作目录。 如果您指定第二个参数文件夹，`resolve` 将使用第一个作为第二个的基础：

```js
path.resolve('tmp', 'joe.txt'); // '/Users/joe/tmp/joe.txt' if run from my home folder
```

如果第一个参数以斜杠开头，则表示它是绝对路径：

```js
path.resolve('/etc', 'joe.txt'); // '/etc/joe.txt'
```

`path.normalize()` 是另一个有用的函数，当它包含诸如 `.` 或 `..` 之类的相对说明符或双斜杠时，它将尝试计算实际路径：

```js
path.normalize('/users/joe/..//test.txt'); // '/users/test.txt'
```

**resolve 和 normalize 都不会检查路径是否存在**。 他们只是根据获得的信息计算出一条路径。