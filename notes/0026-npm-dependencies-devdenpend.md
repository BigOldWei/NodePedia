# npm 依赖项和 devDependencies

当你使用 `npm install <package-name>` 安装一个 npm 包时，你将它安装为一个**依赖**。

该包会自动列在 package.json 文件中的 `dependencies` 列表下（从 npm 5 开始，您必须手动指定 `--save`）。

当您添加 `-D` 标志或 `--save-dev` 时，会将其安装为开发依赖项，这会将其添加到 `devDependencies` 列表中。

开发依赖项旨在作为仅开发包，在生产中不需要。 例如测试包、webpack 或 Babel。

当你进入生产环境时，如果你输入 `npm install` 并且该文件夹包含一个 `package.json` 文件，它们就会被安装，因为 npm 假设这是一个开发部署。

您需要设置 `--production` 标志 (`npm install --production`) 以避免安装这些开发依赖项。
