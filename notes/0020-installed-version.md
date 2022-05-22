# 查找已安装的 npm 包版本

要查看所有已安装 npm 包的版本，包括它们的依赖项：

```bash
npm list
```

例如：

```bash
❯ npm list
/Users/joe/dev/node/cowsay
└─┬ cowsay@1.3.1
  ├── get-stdin@5.0.1
  ├─┬ optimist@0.6.1
  │ ├── minimist@0.0.10
  │ └── wordwrap@0.0.3
  ├─┬ string-width@2.1.1
  │ ├── is-fullwidth-code-point@2.0.0
  │ └─┬ strip-ansi@4.0.0
  │   └── ansi-regex@3.0.0
  └── strip-eof@1.0.0
```

您也可以只打开 `package-lock.json` 文件，但这涉及一些视觉扫描。

`npm list -g` 是相同的，但用于全局安装的包。

要仅获取您的顶级软件包（基本上是您告诉 npm 安装并在 `package.json` 中列出的那些），请运行 `npm list --depth=0`：

```bash
❯ npm list --depth=0
/Users/joe/dev/node/cowsay
└── cowsay@1.3.1
```

您可以通过指定其名称来获取特定包的版本：

```bash
❯ npm list cowsay
/Users/joe/dev/node/cowsay
└── cowsay@1.3.1
```

这也适用于您安装的软件包的依赖项：

```bash
❯ npm list minimist
/Users/joe/dev/node/cowsay
└─┬ cowsay@1.3.1
  └─┬ optimist@0.6.1
    └── minimist@0.0.10
```

如果您想查看 npm 存储库中软件包的最新可用版本，请运行 `npm view [package_name] version`：

```bash
❯ npm view cowsay version
1.3.1
```
