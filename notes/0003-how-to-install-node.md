# 如何安装Node.js

Node.js可以通过不同的方式进行安装。这篇文章重点介绍了最常见和最方便的。

所有主要平台的官方软件包都可以在 https://nodejs.dev/download/ 获得：

![image-20220522192527227](http://img.weidawang.site/i/2022/05/22/628a1e1fa37e8.png)

选择适合你平台的安装包，按照软件提示执行安装即可。

安装完成后，实用以下命令测试是否成功：

```console
node -v  #查看node版本
npm -v	#查看npm版本
```

![image-20220522193011797](http://img.weidawang.site/i/2022/05/22/628a1f3c2aa3d.png)

输出结果如上图即表示安装成功。

安装Node.js一种非常方便的方法是通过包管理器。在这种情况下，每种操作系统都有自己的包管理器。

MacOS、Linux 和 Windows 的其他包管理器在 https://nodejs.dev/download/package-manager/ 中列出

`nvm`是运行 Node.js 的一种流行方式。它允许您轻松切换Node.js版本，并安装新版本以尝试在出现问题时轻松回滚。

使用旧的 Node.js 版本测试代码也非常有用。

有关此选项[的详细信息，请参阅 https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)。

在任何情况下，安装 Node.js 后，您都可以在命令行中访问Node可执行程序。