# package.json 指南

如果您使用 JavaScript，或者您曾经与 JavaScript 项目、Node.js 或前端项目进行过交互，那么您肯定会遇到 `package.json` 文件。

那个有什么用途？ 你应该知道什么，你可以用它做什么很酷的事情？

`package.json` 文件是您项目的清单。 它可以做很多事情，完全不相关。 例如，它是工具配置的中央存储库。 它也是 `npm` 和 `yarn` 存储所有已安装包的名称和版本的地方。

## 文件结构

这是一个示例 package.json 文件：

```json
{ }
```

它是空的！ 对于应用程序，`package.json` 文件中的内容没有固定要求。 唯一的要求是它尊重 JSON 格式，否则尝试以编程方式访问其属性的程序无法读取它。

如果你正在构建一个想要通过 npm 分发的 Node.js 包，事情就会发生根本性的变化，你必须拥有一组可以帮助其他人使用它的属性。 稍后我们将看到更多关于此的内容。

这是另一个 package.json：

```json
{
  "name": "test-project"
}
```

它定义了一个 `name` 属性，该属性告诉应用程序或包的名称，该名称包含在该文件所在的同一文件夹中。

这是一个更复杂的示例，它是从示例 Vue.js 应用程序中提取的：

```json
{
  "name": "test-project",
  "version": "1.0.0",
  "description": "A Vue.js project",
  "main": "src/main.js",
  "private": true,
  "scripts": {
    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    "start": "npm run dev",
    "unit": "jest --config test/unit/jest.conf.js --coverage",
    "test": "npm run unit",
    "lint": "eslint --ext .js,.vue src test/unit",
    "build": "node build/build.js"
  },
  "dependencies": {
    "vue": "^2.5.2"
  },
  "devDependencies": {
    "autoprefixer": "^7.1.2",
    "babel-core": "^6.22.1",
    "babel-eslint": "^8.2.1",
    "babel-helper-vue-jsx-merge-props": "^2.0.3",
    "babel-jest": "^21.0.2",
    "babel-loader": "^7.1.1",
    "babel-plugin-dynamic-import-node": "^1.2.0",
    "babel-plugin-syntax-jsx": "^6.18.0",
    "babel-plugin-transform-es2015-modules-commonjs": "^6.26.0",
    "babel-plugin-transform-runtime": "^6.22.0",
    "babel-plugin-transform-vue-jsx": "^3.5.0",
    "babel-preset-env": "^1.3.2",
    "babel-preset-stage-2": "^6.22.0",
    "chalk": "^2.0.1",
    "copy-webpack-plugin": "^4.0.1",
    "css-loader": "^0.28.0",
    "eslint": "^4.15.0",
    "eslint-config-airbnb-base": "^11.3.0",
    "eslint-friendly-formatter": "^3.0.0",
    "eslint-import-resolver-webpack": "^0.8.3",
    "eslint-loader": "^1.7.1",
    "eslint-plugin-import": "^2.7.0",
    "eslint-plugin-vue": "^4.0.0",
    "extract-text-webpack-plugin": "^3.0.0",
    "file-loader": "^1.1.4",
    "friendly-errors-webpack-plugin": "^1.6.1",
    "html-webpack-plugin": "^2.30.1",
    "jest": "^22.0.4",
    "jest-serializer-vue": "^0.3.0",
    "node-notifier": "^5.1.2",
    "optimize-css-assets-webpack-plugin": "^3.2.0",
    "ora": "^1.2.0",
    "portfinder": "^1.0.13",
    "postcss-import": "^11.0.0",
    "postcss-loader": "^2.0.8",
    "postcss-url": "^7.2.1",
    "rimraf": "^2.6.0",
    "semver": "^5.3.0",
    "shelljs": "^0.7.6",
    "uglifyjs-webpack-plugin": "^1.1.1",
    "url-loader": "^0.5.8",
    "vue-jest": "^1.0.2",
    "vue-loader": "^13.3.0",
    "vue-style-loader": "^3.0.1",
    "vue-template-compiler": "^2.5.2",
    "webpack": "^3.6.0",
    "webpack-bundle-analyzer": "^2.9.0",
    "webpack-dev-server": "^2.9.1",
    "webpack-merge": "^4.1.0"
  },
  "engines": {
    "node": ">= 6.0.0",
    "npm": ">= 3.0.0"
  },
  "browserslist": ["> 1%", "last 2 versions", "not ie <= 8"]
}
```

这里有*很多*发生的事情：

- `version` 表示当前版本
- `name` 设置应用程序/包名称
- `description` 是应用程序/包的简要描述
- `main` 设置应用程序的入口点
- `private` 如果设置为 `true` 防止应用程序/包被意外发布到 `npm`
- `scripts` 定义了一组可以运行的Node脚本
- `dependencies` 设置作为依赖项安装的`npm` 包列表
- `devDependencies` 设置作为开发依赖项安装的`npm` 包列表
- `engines` 设置此包/应用程序适用于哪些版本的 Node.js
- `browserslist` 用于告诉您要支持哪些浏览器（及其版本）

所有这些属性都由 `npm` 或我们可以使用的其他工具使用。

## 属性细分

本节详细介绍了您可以使用的属性。 我们指的是“包”，但同样适用于您不用作包的本地应用程序。

这些属性中的大多数仅在 https://www.npmjs.com/ 上使用，其他属性由与您的代码交互的脚本使用，例如 `npm` 或其他。

### name

设置包名。

例子：

```json
"name": "test-project"
```

名称必须少于 214 个字符，不能有空格，只能包含小写字母、连字符 (`-`) 或下划线 (`_`)。

这是因为当一个包在 `npm` 上发布时，它会根据这个属性获得自己的 URL。

如果您在 GitHub 上公开发布此包，则此属性的一个很好的值是 GitHub 存储库名称。

### author

列出包作者姓名

例子：

```json
{
  "author": "Joe <joe@whatever.com> (https://whatever.com)"
}
```

也可以使用这种格式：

```json
{
  "author": {
    "name": "Joe",
    "email": "joe@whatever.com",
    "url": "https://whatever.com"
  }
}
```

### contributors

除了作者之外，该项目还可以有一个或多个贡献者。 该属性是一个列出它们的数组。

例子：

```json
{
  "contributors": ["Joe <joe@whatever.com> (https://whatever.com)"]
}
```

也可以使用这种格式：

```json
{
  "contributors": [
    {
      "name": "Joe",
      "email": "joe@whatever.com",
      "url": "https://whatever.com"
    }
  ]
}
```

### bugs

包问题跟踪器的链接，很可能是 GitHub 问题页面

例子：

```json
{
  "bugs": "https://github.com/whatever/package/issues"
}
```

### homepage

设置包主页

例子：

```json
{
  "homepage": "https://whatever.com/package"
}
```

### version

表示包的当前版本。

例子：

```json
"version": "1.0.0"
```

此属性遵循版本的语义版本控制 (semver) 表示法，这意味着版本始终用 3 个数字表示：`x.x.x`。

第一个数字是主要版本，第二个是次要版本，第三个是补丁版本。

这些数字有一个含义：仅修复错误的版本是补丁版本，引入向后兼容更改的版本是次要版本，主要版本可以有重大更改。

### license

表示包的许可证。

例子：

```json
"license": "MIT"
```

### keywords

此属性包含与您的包的功能相关联的关键字数组。

例子：

```json
"keywords": [
  "email",
  "machine learning",
  "ai"
]
```

这有助于人们在浏览类似包或浏览 https://www.npmjs.com/ 网站时找到您的包。

### description

此属性包含包的简要说明

例子：

```json
"description": "A package to work with strings"
```

如果您决定将包发布到 `npm` 以便人们可以了解包的内容，这将特别有用。

### repository

此属性指定此包存储库所在的位置。

例子：

```json
"repository": "github:whatever/testing",
```

注意 `github` 前缀。 还有其他受欢迎的服务：

```json
"repository": "gitlab:whatever/testing",

"repository": "bitbucket:whatever/testing",
```

您可以显式设置版本控制系统：

```json
"repository": {
  "type": "git",
  "url": "https://github.com/whatever/testing.git"
}
```

您可以使用不同的版本控制系统：

```json
"repository": {
  "type": "svn",
  "url": "..."
}
```

### main

设置包的入口点。

当您在应用程序中导入此包时，应用程序将在其中搜索模块导出。

例子：

```json
"main": "src/main.js"
```

### private

如果设置为 `true` 防止应用程序/包被意外发布到 `npm`

例子：

```json
"private": true
```

### scripts

定义一组可以运行的Node脚本

例子：

```json
"scripts": {
  "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
  "start": "npm run dev",
  "unit": "jest --config test/unit/jest.conf.js --coverage",
  "test": "npm run unit",
  "lint": "eslint --ext .js,.vue src test/unit",
  "build": "node build/build.js"
}
```

这些脚本是命令行应用程序。 您可以通过调用 `npm run XXXX` 或 `yarn XXXX` 来运行它们，其中 `XXXX` 是命令名称。 示例：`npm run dev`。

您可以为命令使用任何您想要的名称，并且脚本可以执行任何您想要的操作。

### dependencies

设置作为依赖项安装的“npm”包列表。

使用 npm 或 yarn 安装包时：

```bash
npm install <PACKAGENAME>
yarn add <PACKAGENAME>
```

该软件包会自动插入此列表中。

例子：

```json
"dependencies": {
  "vue": "^2.5.2"
}
```

### devDependencies

设置作为开发依赖项安装的“npm”包列表。

它们与“依赖项”不同，因为它们只能安装在开发机器上，而不需要在生产中运行代码。

使用 npm 或 yarn 安装包时：

```bash
npm install --save-dev <PACKAGENAME>
yarn add --dev <PACKAGENAME>
```

该软件包会自动插入此列表中。

例子：

```json
"devDependencies": {
  "autoprefixer": "^7.1.2",
  "babel-core": "^6.22.1"
}
```

### engines

设置此包/应用程序使用的 Node.js 版本和其他命令

例子：

```json
"engines": {
  "node": ">= 6.0.0",
  "npm": ">= 3.0.0",
  "yarn": "^0.13.0"
}
```

### browserslist

用于告诉您要支持哪些浏览器（及其版本）。 它被 Babel、Autoprefixer 和其他工具引用，仅将 polyfills 和 fallbacks 添加到您的目标浏览器中。

例子：

```json
"browserslist": [
  "> 1%",
  "last 2 versions",
  "not ie <= 8"
]
```

此配置意味着您希望以至少 1% 的使用率支持所有浏览器的最后 2 个主要版本（来自 [CanIUse.com](https://caniuse.com/) 统计数据），IE8 和更低版本除外。

([查看更多](https://www.npmjs.com/package/browserslist))

### 命令特定的属性

`package.json` 文件还可以托管特定于命令的配置，例如 Babel、ESLint 等。

每个都有一个特定的属性，如 `eslintConfig`、`babel` 等。 这些是特定于命令的，您可以在相应的命令/项目文档中找到如何使用它们。

## 软件包版本

你已经在上面的描述中看到了这样的版本号：`~3.0.0` 或 `^0.13.0`。 它们是什么意思，您可以使用哪些其他版本说明符？

该符号指定您的包从该依赖项接受哪些更新。

鉴于使用 semver（语义版本控制）所有版本都有 3 位数字，第一个是主要版本，第二个是次要版本，第三个是补丁版本，你有这些 "[Rules](https://nodejs.dev/ 学习/语义版本控制使用 npm/)"。

您可以组合范围内的大多数版本，如下所示：`1.0.0 || >=1.1.0 <1.2.0`，使用 1.0.0 或 1.1.0 以上的一个版本，但低于 1.2.0。

