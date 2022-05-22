# 在 Node.js 中接受来自命令行的输入

如何使 Node.js CLI 程序具有交互性？

Node.js 从版本 7 开始提供 [`readline` 模块](https://nodejs.org/api/readline.html) 来执行此操作：从可读流（例如 `process.stdin` 流）获取输入， 在 Node.js 程序的执行过程中，它是终端输入，一次一行。

```js
const readline = require('readline').createInterface({
  input: process.stdin,
  output: process.stdout,
});

readline.question(`What's your name?`, name => {
  console.log(`Hi ${name}!`);
  readline.close();
});
```

这段代码询问用户的 *name*，一旦输入文本并且用户按下回车键，我们就会发送问候语。

`question()` 方法显示第一个参数（一个问题）并等待用户输入。 一旦按下回车，它就会调用回调函数。

在这个回调函数中，我们关闭了 readline 接口。

`readline` 提供了其他几种方法，请在上面链接的包文档中查看它们。

如果您需要密码，最好不要回显它，而是显示一个“*”符号。

最简单的方法是使用 [`readline-sync` 包](https://www.npmjs.com/package/readline-sync)，它在 API 方面非常相似，并且开箱即用。

[Inquirer.js 包](https://github.com/SBoudrias/Inquirer.js) 提供了更完整和抽象的解决方案。

你可以使用 npm installinquirer 安装它，然后你可以像这样复制上面的代码：

```js
const inquirer = require('inquirer');

const questions = [
  {
    type: 'input',
    name: 'name',
    message: "What's your name?",
  },
];

inquirer.prompt(questions).then(answers => {
  console.log(`Hi ${answers.name}!`);
});
```

Inquirer.js 可以让你做很多事情，比如询问多项选择、有单选按钮、确认等等。

值得了解所有替代方案，尤其是 Node.js 提供的内置替代方案，但如果您打算将 CLI 输入提升到一个新的水平，Inquirer.js 是最佳选择。