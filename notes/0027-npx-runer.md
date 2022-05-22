# npx Node.js 包运行器

`npx` 是一个非常强大的命令，在 2017 年 7 月发布的 **npm** 开始版本 5.2 中提供。

> 如果不想安装npm，可以[将npx安装为独立包](https://www.npmjs.com/package/npx)

`npx` 允许您运行使用 Node.js 构建并通过 npm 注册表发布的代码。

## Easily run local commands

Node.js 开发人员过去将大多数可执行命令发布为全局包，以便它们位于路径中并立即可执行。

这很痛苦，因为您无法真正安装同一命令的不同版本。

运行 `npx commandname` 会自动在项目的 `node_modules` 文件夹中找到正确的命令引用，而无需知道确切的路径，也不需要将包安装在全局和用户的路径中。

## 免安装命令执行

`npx` 还有一个很棒的特性，它允许在不先安装命令的情况下运行命令。

这非常有用，主要是因为：

1.你不需要安装任何东西
2. 你可以运行同一命令的不同版本，使用语法@version

使用 `npx` 的典型演示是通过 `cowsay` 命令。 `cowsay` 将打印一头牛，说出你在命令中写的内容。 例如：

`cowsay "Hello"` 将打印

```console
 _______
< Hello >
 -------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

这仅适用于您之前从 npm 全局安装的 `cowsay` 命令。 否则，当您尝试运行该命令时会出现错误。

`npx` 允许您运行该 npm 命令而无需先安装它。 如果找不到该命令，`npx` 会将其安装到中央缓存中：

```bash
npx cowsay "Hello"
```

将完成这项工作。

现在，这是一个有趣的无用命令。 其他场景包括：

- 运行 `vue` CLI 工具来创建新的应用程序并运行它们：`npx @vue/cli create my-vue-app`
- 使用`create-react-app` 创建一个新的 React 应用程序：`npx create-react-app my-react-app`

还有很多。

## 使用不同的 Node.js 版本运行一些代码

使用 `@` 指定版本，并将其与 [`node` npm 包](https://www.npmjs.com/package/node) 结合使用：

```bash
npx node@10 -v #v10.18.1
npx node@12 -v #v12.14.1
```

这有助于避免使用 `nvm` 之类的工具或其他 Node.js 版本管理工具。

## 直接从 URL 运行任意代码片段

`npx` 并不限制您使用在 npm 注册表上发布的包。

您可以运行位于 GitHub gist 中的代码，例如：

```bash
npx https://gist.github.com/zkat/4bc19503fe9e9309e2bfaa2c58074d32
```
当然，在运行不受控制的代码时需要小心，因为权力越大，责任越大。