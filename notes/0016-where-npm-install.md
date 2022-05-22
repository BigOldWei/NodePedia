# npm 在哪里安装软件包？

当你使用 `npm` 安装一个包时，你可以执行 2 种类型的安装：

- 本地安装
- 全局安装

默认情况下，当您键入“npm install”命令时，例如：

```bash
npm install lodash
```

该软件包安装在当前文件树的 `node_modules` 子文件夹下。

发生这种情况时，`npm` 还会在当前文件夹中的 `package.json` 文件的 `dependencies` 属性中添加 `lodash` 条目。

使用 `-g` 标志执行全局安装：

```bash
npm install -g lodash
```

发生这种情况时，npm 不会在本地文件夹下安装包，而是使用全局位置。

究竟在哪里？

`npm root -g` 命令将告诉您该确切位置在您的机器上的什么位置。

在 macOS 或 Linux 上，此位置可能是 `/usr/local/lib/node_modules`。 在 Windows 上，它可能是 `C:\Users\YOU\AppData\Roaming\npm\node_modules`

但是，如果您使用 `nvm` 来管理 Node.js 版本，则该位置会有所不同。

例如，如果您的用户名是“joe”并且您使用“nvm”，那么包位置将显示为“/Users/joe/.nvm/versions/node/v8.9.0/lib/node_modules”。