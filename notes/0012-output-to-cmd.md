# 使用 Node.js 输出到命令行

## 使用console模块的基本输出

Node.js 提供了一个 [`console` 模块](https://nodejs.org/api/console.html)，它提供了大量非常有用的方式来与命令行交互。

它与您在浏览器中找到的 `console` 对象基本相同。

最基本和最常用的方法是 `console.log()`，它将您传递给它的字符串打印到控制台。

如果您传递一个对象，它将把它呈现为一个字符串。

您可以将多个变量传递给 `console.log`，例如：

```js
const x = 'x';
const y = 'y';
console.log(x, y);
```

和 Node.js 将打印两者。

我们还可以通过传递变量和格式说明符来格式化漂亮的短语。

例如：

```js
console.log('My %s has %d ears', 'cat', 2);
```

- `%s` 将变量格式化为字符串
- `%d` 将变量格式化为数字
- `%i` 仅将变量格式化为其整数部分
- `%o` 将变量格式化为对象

举例：

```js
console.log('%o', Number);
```

## 清除控制台

`console.clear()` 清除控制台（行为可能取决于使用的控制台）

## 计数元素

`console.count()` 是一个方便的方法。

用这个代码：

```js
const x = 1
const y = 2
const z = 3
console.count(
  'The value of x is ' + x + 
  ' and has been checked .. how many times?'
)
console.count(
  'The value of x is ' + x + 
  ' and has been checked .. how many times?'
)
console.count(
  'The value of y is ' + y + 
  ' and has been checked .. how many times?'
)
```

发生的情况是 `console.count()` 将计算字符串被打印的次数，并在其旁边打印计数：

你可以只数苹果和橙子：

```js
const oranges = ['orange', 'orange'];
const apples = ['just one apple'];
oranges.forEach(fruit => {
  console.count(fruit);
});
apples.forEach(fruit => {
  console.count(fruit);
});
```

## 重置计数

console.countReset() 方法重置与 console.count() 一起使用的计数器。

我们将使用苹果和橙子的例子来证明这一点。

```js
const oranges = ['orange', 'orange'];
const apples = ['just one apple'];
oranges.forEach(fruit => {
  console.count(fruit);
});
apples.forEach(fruit => {
  console.count(fruit);
});

console.countReset('orange');

oranges.forEach(fruit => {
  console.count(fruit);
});
```

请注意对 `console.countReset('orange')` 的调用如何将值计数器重置为零。

## 打印堆栈跟踪

在某些情况下，打印函数的调用堆栈跟踪很有用，也许是为了回答问题*您是如何到达代码的那部分的？*

您可以使用 `console.trace()` 执行此操作：

```js
const function2 = () => console.trace();
const function1 = () => function2();
function1();
```

这将打印堆栈跟踪。 如果我们在 Node.js REPL 中尝试，这就是打印的内容：

```bash
Trace
    at function2 (repl:1:33)
    at function1 (repl:1:25)
    at repl:1:1
    at ContextifyScript.Script.runInThisContext (vm.js:44:33)
    at REPLServer.defaultEval (repl.js:239:29)
    at bound (domain.js:301:14)
    at REPLServer.runBound [as eval] (domain.js:314:12)
    at REPLServer.onLine (repl.js:440:10)
    at emitOne (events.js:120:20)
    at REPLServer.emit (events.js:210:7)
```

## 计算花费的时间

您可以使用 `time()` 和 `timeEnd()` 轻松计算函数运行所需的时间

```js
const doSomething = () => console.log('test');
const measureDoingSomething = () => {
  console.time('doSomething()');
  // do something, and measure the time it takes
  doSomething();
  console.timeEnd('doSomething()');
};
measureDoingSomething();
```

## 标准输出和标准错误

正如我们所见，console.log 非常适合在控制台中打印消息。 这就是所谓的标准输出，或“stdout”。

`console.error` 打印到 `stderr` 流。

它不会出现在控制台中，但会出现在错误日志中。

## 为输出着色

您可以使用 [转义序列](https://gist.github.com/iamnewton/8754917) 在控制台中为文本输出着色。 转义序列是一组标识颜色的字符。

例子：

```js
console.log('\x1b[33m%s\x1b[0m', 'hi!');
```

你可以在 Node.js REPL 中尝试，它会以黄色打印 `hi!`。

但是，这是执行此操作的低级方法。 为控制台输出着色的最简单方法是使用库。 [Chalk](https://github.com/chalk/chalk) 就是这样一个库，除了着色之外，它还有助于其他样式工具，例如使文本变为粗体、斜体或下划线。

你用 `npm install chalk@4` 安装它，然后你就可以使用它了：

```js
const chalk = require('chalk');
console.log(chalk.yellow('hi!'));
```

使用 `chalk.yellow` 比尝试记住转义码要方便得多，而且代码的可读性也更高。

检查上面发布的项目链接以获取更多使用示例。

## 创建进度条

[Progress](https://www.npmjs.com/package/progress) 是一个很棒的包，用于在控制台中创建进度条。 使用 `npm install progress` 安装它

此代码段创建一个 10 步进度条，每 100 毫秒完成一个步骤。 当柱完成时，我们清除间隔：

```js
const ProgressBar = require('progress');

const bar = new ProgressBar(':bar', { total: 10 });
const timer = setInterval(() => {
  bar.tick();
  if (bar.complete) {
    clearInterval(timer);
  }
}, 100);
```
