# 卸载 npm 包

要卸载您之前在**本地**安装的包（使用`node_modules`文件夹中的`npm install <package-name>`，运行

```bash
npm uninstall <package-name>
```

从项目根文件夹（包含 node_modules 文件夹的文件夹）。

使用 `-S` 标志或 `--save`，此操作还将删除 `package.json` 文件中的引用。

卸载 npm 包后，package.json 将使用 devDependency 和依赖项自动更新。

如果包是 **globally** 安装的，则需要添加 `-g` / `--global` 标志：

```bash
npm uninstall -g <package-name>
```

例如：

```bash
npm uninstall -g webpack
```

并且您可以从系统上的任何位置运行此命令，因为您当前所在的文件夹无关紧要。
