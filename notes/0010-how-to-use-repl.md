# 如何使用 Node.js REPL

`node` 命令是我们用来运行 Node.js 脚本的命令：

```bash
node script.js
```

如果我们在没有任何要执行的脚本或没有任何参数的情况下运行 `node` 命令，我们将启动一个 REPL 会话：

```bash
node
```

> 注意：REPL 代表 Read Evaluate Print Loop，它是一个编程语言环境（基本上是一个控制台窗口），将单个表达式作为用户输入，并在执行后将结果返回到控制台。 REPL 会话提供了一种快速测试简单 JavaScript 代码的便捷方式。

如果您现在在终端中尝试，会发生以下情况：

```bash
❯ node
❯
```

该命令保持空闲模式并等待我们输入内容。

> 提示：如果您不确定如何打开终端，请搜索“如何在 \<your-operating-system> 上打开终端”。

REPL 正在等待我们输入一些 JavaScript 代码，更准确地说。

从简单开始并输入

```console
> console.log('test')
test
undefined
> 
```

第一个值`test`是我们告诉控制台打印的输出，然后我们得到`undefined`，它是运行`console.log()`的返回值。 Node 读取这行代码，对其进行评估，打印结果，然后返回等待更多代码行。 Node 将为我们在 REPL 中执行的每一段代码循环执行这三个步骤，直到我们退出会话。 这就是 REPL 得名的地方。

Node 会自动打印任何 JavaScript 代码行的结果，而无需指示它这样做。 例如，输入以下行并按 Enter：

```console
> 5 === '5'
false
>
```

请注意上述两行输出的差异。 Node REPL 在执行 `console.log()` 后打印了 `undefined`，而另一方面，它只是打印了 `5 === '5'` 的结果。 你需要记住，前者只是 JavaScript 中的一个语句，而后者是一个表达式。

在某些情况下，您要测试的代码可能需要多行。 例如，假设您要定义一个生成随机数的函数，在 REPL 会话中键入以下行并按 enter：

```console
function generateRandom() {
...
```

Node REPL 足够智能，可以确定您还没有完成编写代码，它会进入多行模式，让您输入更多代码。 现在完成你的函数定义并按回车：

```console
function generateRandom() {
...return Math.random()
}
undefined
```

Node 将退出多行模式，并打印 `undefined`，因为没有返回值。 这种多线模式是有限的。 Node 在 REPL 中提供了一个更具特色的编辑器。 我们在下面的点命令下讨论它。

## 使用tab自动补全

REPL 最酷的地方在于它是交互式的。

在您编写代码时，如果您按下 `tab` 键，REPL 将尝试自动完成您编写的内容以匹配您已经定义或预定义的变量。

## 探索 JavaScript 对象

尝试输入 JavaScript 类的名称，例如“Number”，添加一个点并按“tab”。

REPL 将打印您可以在该类上访问的所有属性和方法：

[![Pressing tab reveals object properties](http://img.weidawang.site/i/2022/05/22/6289c270a80e3.png)](https://nodejs.dev/static/2b60eb9487f93b672da38e391d2e5e56/6937a/tab.png)

## 探索全局对象

你可以通过输入`global.`并按`tab`来检查你可以访问的全局变量：

[![Globals](http://img.weidawang.site/i/2022/05/22/6289c270edb40.png)](https://nodejs.dev/static/c2bef52ca393ecb33846c54af34927a1/6937a/globals.png)

## _ 特殊变量

如果在某些代码之后键入`_`，那将打印最后一次操作的结果。

## 向上箭头键

如果您按“向上”箭头键，您将可以访问在当前甚至以前的 REPL 会话中执行的前几行代码的历史记录。

## .命令

REPL 有一些特殊的命令，都以点 `.` 开头。 他们是

- `.help`：显示点命令帮助
- `.editor`：启用编辑器模式，轻松编写多行 JavaScript 代码。 进入此模式后，输入 ctrl-D 运行您编写的代码。
- `.break`：输入多行表达式时，输入 .break 命令将中止进一步的输入。 与按 ctrl-C 相同。
- `.clear`：将 REPL 上下文重置为空对象并清除当前输入的任何多行表达式。
- `.load`：加载一个 JavaScript 文件，相对于当前工作目录
- `.save`：将您在 REPL 会话中输入的所有内容保存到文件中（指定文件名）
- `.exit`：退出repl（与按两次ctrl-C相同）

REPL 知道您何时键入多行语句，而无需调用 `.editor`。

例如，如果您开始键入这样的迭代：

```console
[1, 2, 3].forEach(num => {
```

然后你按下回车键，REPL 将转到一个以 3 个点开头的新行，表示你现在可以继续在该块上工作。

```console
... console.log(num)
... })
```

如果在行尾键入`.break`，多行模式将停止，语句不会被执行。

## 从 JavaScript 文件运行 REPL

我们可以使用 `repl` 在 JavaScript 文件中导入 REPL。

```js
const repl = require('repl');
```

使用 repl 变量，我们可以执行各种操作。 要启动 REPL 命令提示符，请键入以下行

```js
const local = repl.start(prompt);
```

repl.start() 启动 REPL 环境，提示符是一个字符串，它接受显示 REPL 何时启动的提示符。 默认为'>'，但我们可以定义自定义提示。

在命令行中运行该文件。

```bash
node repl.js
```

```console
>const n = 10
```

您可以在退出 REPL 时显示一条消息

```js
local.on('exit', () => {
  console.log('exiting repl');
  process.exit();
});
```
