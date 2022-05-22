# Node.js 流

## 什么是流

流是支持 Node.js 应用程序的基本概念之一。

它们是一种以有效方式处理读取/写入文件、网络通信或任何类型的端到端信息交换的方法。

流不是 Node.js 独有的概念。它们是几十年前在 Unix 操作系统中引入的，程序可以通过管道运算符 (`|`) 相互传递流。

例如，在传统方式中，当您告诉程序读取文件时，文件会从头到尾读入内存，然后您对其进行处理。

使用流，您可以逐段读取它，处理其内容而不将其全部保存在内存中。

Node.js [`stream` 模块](https://nodejs.org/api/stream.html) 提供了构建所有流 API 的基础。所有流都是 [EventEmitter](https://nodejs.org/api/events.html#events_class_eventemitter) 的实例

## 为什么是流

与使用其他数据处理方法相比，流基本上提供了两个主要优势：

- **内存效率**：您不需要在内存中加载大量数据就可以处理它
- **时间效率**：开始处理数据所需的时间更少，因为您可以在拥有数据后立即开始处理，而不是等到整个数据有效负载可用

## 流的示例

一个典型的例子是从磁盘读取文件。

使用 Node.js `fs` 模块，您可以读取文件，并在与 HTTP 服务器建立新连接时通过 HTTP 提供它：

```js
JScopyconst http = require('http');const fs = require('fs');
const server = http.createServer(function (req, res) {  fs.readFile(`${__dirname}/data.txt`, (err, data) => {    res.end(data);  });});server.listen(3000);
```

`readFile()` 读取文件的全部内容，并在完成后调用回调函数。

回调中的 `res.end(data)` 会将文件内容返回给 HTTP 客户端。

如果文件很大，操作将花费相当多的时间。 这是使用流编写的相同内容：

```js
JScopyconst http = require('http');const fs = require('fs');
const server = http.createServer((req, res) => {  const stream = fs.createReadStream(`${__dirname}/data.txt`);  stream.pipe(res);});server.listen(3000);
```

我们不是等到文件被完全读取，而是在准备好要发送的数据块后立即开始将其流式传输到 HTTP 客户端。

＃＃ 管道（）

上面的例子使用了 `stream.pipe(res)` 行：`pipe()` 方法在文件流上被调用。

这段代码有什么作用？ 它获取源，并将其通过管道传输到目标。

您在源流上调用它，因此在这种情况下，文件流通过管道传输到 HTTP 响应。

`pipe()` 方法的返回值是目标流，这是一件非常方便的事情，可以让我们链接多个 `pipe()` 调用，如下所示：

```js
JScopy
src.pipe(dest1).pipe(dest2);
```

这个构造和做的一样

```js
JScopy
src.pipe(dest1);
dest1.pipe(dest2);
```

## Streams 驱动的 Node.js API

由于它们的优势，许多 Node.js 核心模块都提供了原生流处理能力，最值得注意的是：

- `process.stdin` 返回一个连接到标准输入的流
- `process.stdout` 返回一个连接到标准输出的流
- `process.stderr` 返回一个连接到 stderr 的流
- `fs.createReadStream()` 创建一个文件的可读流
- `fs.createWriteStream()` 创建文件的可写流
- `net.connect()` 启动基于流的连接
- `http.request()` 返回一个 http.ClientRequest 类的实例，它是一个可写流
- `zlib.createGzip()` 使用 gzip（一种压缩算法）将数据压缩成流
- `zlib.createGunzip()` 解压缩 gzip 流。
- `zlib.createDeflate()` 使用 deflate（一种压缩算法）将数据压缩成一个流
- `zlib.createInflate()` 解压一个 deflate 流

## 不同类型的流

有四类流：

- `Readable`：您可以通过管道传输但不能通过管道传输的流（您可以接收数据，但不能向其发送数据）。当您将数据推送到可读流中时，它会被缓冲，直到消费者开始读取数据。
- `Writable`: 一个可以通过管道进入但不能从管道传输的流（你可以发送数据，但不能从中接收数据）
- `Duplex`：一个既可以输入也可以输出的流，基本上是可读和可写流的组合
- `Transform`：Transform 流类似于 Duplex，但输出是其输入的变换

## 如何创建可读流

我们从 [`stream` 模块](https://nodejs.org/api/stream.html) 获取 Readable 流，我们对其进行初始化并实现 `readable._read()` 方法。

首先创建一个流对象：

```js
const Stream = require('stream');
const readableStream = new Stream.Readable();
```

然后实现`_read`：

```js
readableStream._read = () => {};
```

您还可以使用 `read` 选项实现 `_read`：

```js
const readableStream = new Stream.Readable({
  read() {},
});
```

Now that the stream is initialized, we can send data to it:

```js
readableStream.push('hi!');
readableStream.push('ho!');
```

## 如何创建可写流

为了创建一个可写流，我们扩展了基本的 `Writable` 对象，并实现了它的 _write() 方法。

首先创建一个流对象：

```js
const Stream = require('stream');
const writableStream = new Stream.Writable();
```

然后实现`_write`：

```js
writableStream._write = (chunk, encoding, next) => {
  console.log(chunk.toString());
  next();
};
```

您现在可以将可读流通过管道传输到：

```js
process.stdin.pipe(writableStream);
```

## 如何从可读流中获取数据

我们如何从可读流中读取数据？ 使用可写流：

```js
const Stream = require('stream');
const readableStream = new Stream.Readable({  read() {},});const writableStream = new Stream.Writable();
writableStream._write = (chunk, encoding, next) => {  console.log(chunk.toString());  next();};
readableStream.pipe(writableStream);
readableStream.push('hi!');readableStream.push('ho!');
```

您还可以使用 `readable` 事件直接使用可读流：

```js
readableStream.on('readable', () => {
  console.log(readableStream.read());
});
```

## 如何将数据发送到可写流

使用流`write()`方法：

```js
writableStream.write('hey!\n');
```

## 发信号通知您结束写入的可写流

使用 `end()` 方法：

```js
const Stream = require('stream');
const readableStream = new Stream.Readable({  read() {},});const writableStream = new Stream.Writable();
writableStream._write = (chunk, encoding, next) => {  console.log(chunk.toString());  next();};
readableStream.pipe(writableStream);
readableStream.push('hi!');readableStream.push('ho!');
readableStream.on('close', () => writableStream.end());writableStream.on('close', () => console.log('ended'));
readableStream.destroy();
```

在上面的示例中，在可读流上的 `close` 事件的侦听器中调用 `end()` 以确保在所有写入事件都通过管道之前不会调用它，因为这样做会导致 `error` 要发出的事件。 在可读流上调用 `destroy()` 会导致发出 `close` 事件。 可写流上的 `close` 事件的侦听器演示了该过程的完成，因为它是在调用 `end()` 之后发出的。

## 如何创建转换流

我们从 [`stream` 模块](https://nodejs.org/api/stream.html) 中获取 Transform 流，并对其进行初始化并实现 `transform._transform()` 方法。

首先创建一个变换流对象：

```js
const { Transform } = require('stream');
const transformStream = new Transform();
```

然后实现`_transform`：

```js
transformStream._transform = (chunk, encoding, callback) => {
  transformStream.push(chunk.toString().toUpperCase());
  callback();
};
```

管道可读流：

```js
process.stdin.pipe(transformStream).pipe(process.stdout);
```
