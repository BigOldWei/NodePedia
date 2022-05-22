# Node.js os 模块

该模块提供了许多功能，您可以使用这些功能从底层操作系统和程序运行的计算机中检索信息，并与之交互。

```js
const os = require('os');
```

有一些有用的属性可以告诉我们一些与处理文件相关的关键信息：

`os.EOL` 给出了行分隔符序列。 在 Linux 和 macOS 上是 `\n`，在 Windows 上是 `\r\n`。

`os.constants.signals` 告诉我们所有与处理进程信号相关的常量，如 SIGHUP、SIGKILL 等。

`os.constants.errno` 设置错误报告的常量，如 EADDRINUSE、EOVERFLOW 等。

你可以在 https://nodejs.org/api/os.html#os_signal_constants 上阅读它们。

现在让我们看看 `os` 提供的主要方法：

## `os.arch()`

返回标识底层架构的字符串，如`arm`、`x64`、`arm64`。

## `os.cpus()`

返回有关系统上可用 CPU 的信息。

例子：

```js
[
  {
    model: 'Intel(R) Core(TM)2 Duo CPU     P8600  @ 2.40GHz',
    speed: 2400,
    times: {
      user: 281685380,
      nice: 0,
      sys: 187986530,
      idle: 685833750,
      irq: 0,
    },
  },
  {
    model: 'Intel(R) Core(TM)2 Duo CPU     P8600  @ 2.40GHz',
    speed: 2400,
    times: {
      user: 282348700,
      nice: 0,
      sys: 161800480,
      idle: 703509470,
      irq: 0,
    },
  },
];
```

## `os.endianness()`

返回 `BE` 或 `LE` 取决于 Node.js 是使用 [Big Endian 还是 Little Endian] (https://en.wikipedia.org/wiki/Endianness) 编译的。

## `os.freemem()`

返回表示系统中可用内存的字节数。

## `os.homedir()`

返回当前用户主目录的路径。

例子：

```js
'/Users/joe';
```

## `os.hostname()`

返回主机名。

## `os.loadavg()`

返回操作系统对平均负载的计算。

它只在 Linux 和 macOS 上返回一个有意义的值。

例子：

```js
[3.68798828125, 4.00244140625, 11.1181640625];
```

## `os.networkInterfaces()`

返回系统上可用网络接口的详细信息。

例子：

```console
{ lo0:
   [ { address: '127.0.0.1',
       netmask: '255.0.0.0',
       family: 'IPv4',
       mac: 'fe:82:00:00:00:00',
       internal: true },
     { address: '::1',
       netmask: 'ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff',
       family: 'IPv6',
       mac: 'fe:82:00:00:00:00',
       scopeid: 0,
       internal: true },
     { address: 'fe80::1',
       netmask: 'ffff:ffff:ffff:ffff::',
       family: 'IPv6',
       mac: 'fe:82:00:00:00:00',
       scopeid: 1,
       internal: true } ],
  en1:
   [ { address: 'fe82::9b:8282:d7e6:496e',
       netmask: 'ffff:ffff:ffff:ffff::',
       family: 'IPv6',
       mac: '06:00:00:02:0e:00',
       scopeid: 5,
       internal: false },
     { address: '192.168.1.38',
       netmask: '255.255.255.0',
       family: 'IPv4',
       mac: '06:00:00:02:0e:00',
       internal: false } ],
  utun0:
   [ { address: 'fe80::2513:72bc:f405:61d0',
       netmask: 'ffff:ffff:ffff:ffff::',
       family: 'IPv6',
       mac: 'fe:80:00:20:00:00',
       scopeid: 8,
       internal: false } ] }
```

## `os.platform()`

返回编译 Node.js 的平台：

- `达尔文`
- `freebsd`
- `linux`
- `openbsd`
- `win32`
- ...more

## `os.release()`

返回一个标识操作系统版本号的字符串

## `os.tmpdir()`

返回分配的临时文件夹的路径。

## `os.totalmem()`

返回表示系统中可用总内存的字节数。

## `os.type()`

识别操作系统：

- `Linux`
- macOS 上的“达尔文”
- Windows 上的“Windows_NT”

## `os.uptime()`

返回计算机自上次重新启动以来已运行的秒数。

## `os.userInfo()`

返回一个包含当前“username”、“uid”、“gid”、“shell”和“homedir”的对象
