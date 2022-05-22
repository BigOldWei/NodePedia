# 如何使用或执行使用 npm 安装的包

当你使用 `npm` 或者全局安装一个包到你的 `node_modules` 文件夹时，你如何在你的 Node.js 代码中使用它？

假设您安装了流行的 JavaScript 实用程序库 `lodash`，使用

```bash
npm install lodash
```

这将在本地 `node_modules` 文件夹中安装包。

要在您的代码中使用它，您只需使用 `require` 将其导入程序：

```js
const _ = require('lodash');
```

如果你的包是可执行文件怎么办？

在这种情况下，它会将可执行文件放在 `node_modules/.bin/` 文件夹下。

证明这一点的一种简单方法是 [cowsay](https://www.npmjs.com/package/cowsay)。

cowsay 包提供了一个命令行程序，可以执行该程序来让牛说话（以及其他动物🦊）。

当你使用 `npm install cowsay` 安装包时，它会在 `node_modules` 文件夹中安装自己和一些依赖项：

[![The node_modules folder content](http://img.weidawang.site/i/2022/05/22/6289d67c6f6ee.png)](https://nodejs.dev/static/b245c50f5080dae16a2525fae0ba2c91/d2c2a/node_modules-content.png)

有一个隐藏的 .bin 文件夹，其中包含指向 cowsay 二进制文件的符号链接：

[![The binary files](http://img.weidawang.site/i/2022/05/22/6289d67c6af72.png)](https://nodejs.dev/static/99830aefa055e247397de544ad7b7744/d2c2a/binary-files.png)

你如何执行这些？

你当然可以输入 `./node_modules/.bin/cowsay` 来运行它，它可以工作，但是最近版本的 `npm`（从 5.2 开始）中包含的 `npx` 是一个更好的选择。 你只需运行：

```bash
npx cowsay take me out of here
```

并且 `npx` 将找到可执行文件的位置。

[![Cow says something](http://img.weidawang.site/i/2022/05/22/6289d67c5eb0d.png)](https://nodejs.dev/static/ad4f3d3a7464bb0f8a2845fe8e6588c2/d2c2a/cow-say.png)