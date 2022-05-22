# 安装旧版本的 npm 包

您可以使用 `@` 语法安装旧版本的 npm 包：

```bash
npm install <package>@<version>
```

举例：

```bash
npm install cowsay
```

安装版本 1.3.1（在撰写本文时）。

安装 1.2.0 版：

```bash
npm install cowsay@1.2.0
```

全局包也可以这样做：

```bash
npm install -g webpack@4.16.4
```

您可能还对列出软件包的所有先前版本感兴趣。 您可以使用 `npm view <package> versions`：

```bash
❯ npm view cowsay versions
[ '1.0.0',  '1.0.1',  '1.0.2',  '1.0.3',  '1.1.0',  '1.1.1',  '1.1.2',  '1.1.3',  '1.1.4',  '1.1.5',  '1.1.6',  '1.1.7',  '1.1.8',  '1.1.9',  '1.2.0',  '1.2.1',  '1.3.0',  '1.3.1' ]
```

