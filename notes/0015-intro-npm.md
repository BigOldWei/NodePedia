# npm包管理器介绍

## npm 简介

`npm` 是 Node.js 的标准包管理器。

据报道，在 2017 年 1 月，npm 注册表中列出了超过 350000 个包，使其成为地球上最大的单一语言代码存储库，您可以确定（几乎！）所有东西都有一个包。

它最初是一种下载和管理 Node.js 包的依赖项的方式，但后来它也成为前端 JavaScript 中使用的工具。

`npm` 做了很多事情。

> [**Yarn**](https://yarnpkg.com/en/) 和 [**pnpm**](https://pnpm.io/) 是 npm cli 的替代品。 您也可以查看它们。

## 下载

`npm` 管理项目依赖项的下载。

### 安装所有依赖项

如果一个项目有一个 `package.json` 文件，通过运行

```bash
npm install
```

它将在 `node_modules` 文件夹中安装项目所需的一切，如果它不存在则创建它。

### 安装单个包

您还可以通过运行安装特定的包

```bash
npm install <package-name>
```

此外，从 npm 5 开始，此命令将 `<package-name>` 添加到 `package.json` 文件 *dependencies*。在版本 5 之前，您需要添加标志 `--save`。

通常你会看到更多的标志被添加到这个命令中：

- `--save-dev` 安装并将条目添加到 `package.json` 文件 *devDependencies*
- `--no-save` 安装但不将条目添加到 `package.json` 文件 *dependencies*
- `--save-optional` 安装并将条目添加到 `package.json` 文件 * optionalDependencies *
- `--no-optional` 将阻止安装可选依赖项

也可以使用标志的简写：

- -S：--保存
- -D：--保存开发
- -O: --save-可选

*devDependencies* 和 *dependencies* 之间的区别在于前者包含开发工具，如测试库，而后者与生产中的应用程序捆绑在一起。

至于 *optionalDependencies* 不同的是，依赖的构建失败不会导致安装失败。但是，处理缺乏依赖性是您的程序的责任。阅读更多关于 [可选依赖项](https://docs.npmjs.com/cli/v7/configuring-npm/package-json#optionaldependencies)。

### 更新包

更新也很容易，通过运行

```bash
npm update
```

`npm` 将检查所有软件包是否有满足您的版本控制约束的较新版本。

您也可以指定要更新的单个包：

```bash
npm update <package-name>
```

## 版本控制

除了普通下载之外，`npm` 还管理 **versioning**，因此您可以指定包的任何特定版本，或者要求高于或低于您需要的版本。

很多时候你会发现一个库只与另一个库的主要版本兼容。

或者最新版本的库中的一个错误，仍然未修复，导致了一个问题。

指定库的显式版本还有助于让每个人都使用相同版本的包，以便整个团队运行相同的版本，直到 `package.json` 文件更新。

在所有这些情况下，版本控制有很大帮助，并且 `npm` 遵循语义版本控制 (semver) 标准。

## 运行任务

package.json 文件支持一种格式，用于指定可以通过使用运行的命令行任务

```bash
npm run <task-name>
```

例如：

```json
{
  "scripts": {
    "start-dev": "node lib/server-development",
    "start": "node lib/server-production"
  }
}
```

使用这个特性来运行 Webpack 是很常见的：

```json
{
  "scripts": {
    "watch": "webpack --watch --progress --colors --config webpack.conf.js",
    "dev": "webpack --progress --colors --config webpack.conf.js",
    "prod": "NODE_ENV=production webpack -p --config webpack.conf.js",
  }
}
```

所以不要输入那些容易忘记或输入错误的长命令，你可以运行

```console
$ npm run watch
$ npm run dev
$ npm run prod
```

