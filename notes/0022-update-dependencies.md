# 将所有 Node.js 依赖项更新到最新版本

## 包如何成为依赖项

当您使用 `npm install <packagename>` 安装软件包时，最新版本会下载到 `node_modules` 文件夹。 在当前文件夹中的 `package.json` 和 `package-lock.json` 中添加了相应的条目。

npm 确定依赖关系并安装它们的最新版本。

假设您安装了 [`cowsay`](https://www.npmjs.com/package/cowsay)，这是一个漂亮的命令行工具，可让您让奶牛说出 *things*。

当您运行 `npm install cowsay` 时，此条目将添加到 `package.json` 文件中：

```json
{
  "dependencies": {
    "cowsay": "^1.3.1"
  }
}
```

这是 `package-lock.json` 的摘录（为了清楚起见，删除了嵌套依赖项）：

```json
{
  "requires": true,
  "lockfileVersion": 1,
  "dependencies": {
    "cowsay": {
      "version": "1.3.1",
      "resolved": "https://registry.npmjs.org/cowsay/-/cowsay-1.3.1.tgz",
      "integrity": "sha512-3PVFe6FePVtPj1HTeLin9v8WyLl+VmM1l1H/5P+BTTDkMAjufp+0F9eLjzRnOHzVAYeIYFF5po5NjRrgefnRMQ==",
      "requires": {
        "get-stdin": "^5.0.1",
        "optimist": "~0.6.1",
        "string-width": "~2.1.1",
        "strip-eof": "^1.0.0"
      }
    }
  }
}
```

现在这 2 个文件告诉我们，我们安装了 cowsay 的 `1.3.1` 版本，我们的 [npm 版本控制规则](https://docs.npmjs.com/about-semantic-versioning) 是 `^1.3.1 `。 这意味着 npm 可以更新到补丁和次要版本：`1.3.2`、`1.4.0` 等等。

如果有新的次要版本或补丁版本，我们键入 `npm update`，安装的版本会更新，`package-lock.json` 文件会努力填充新版本。

从 npm 版本 5.0.0 开始，`npm update` 使用更新的次要或补丁版本更新 `package.json`。 使用 `npm update --no-save` 来防止修改 `package.json`。

要发现新的软件包版本，请使用 `npm outdated`。

以下是存储库中一些过时软件包的列表：

[![outdated packages](http://img.weidawang.site/i/2022/05/22/6289dbd511467.png)](https://nodejs.dev/static/e967736f8d105e64c22d80ce7a42f52a/ee455/outdated-packages.png)

其中一些更新是*主要*版本。 在这里运行 `npm update` 无济于事。 主要版本*从不*以这种方式更新，因为它们（根据定义）引入了重大更改，而 `npm` 想为您省去麻烦。

## 将所有软件包更新到最新版本

利用 [npm-check-updates](https://www.npmjs.com/package/npm-check-updates)，您可以将所有 `package.json` 依赖项升级到最新版本。

1. 全局安装 `npm-check-updates` 包：

   ```bash
   npm install -g npm-check-updates
   ```

2. 现在运行 `npm-check-updates` 来升级 `package.json` 中的所有版本提示，允许安装新的主要版本：

   ```bash
   ncu -u
   ```

3. 最后，运行标准安装：

   ```bash
   npm install
   ```
