# Node 简介

Node.js是一个开源和跨平台的JavaScript运行时环境。它是几乎任何类型的项目的流行工具！

Node.js在浏览器之外运行V8 JavaScript引擎，这是Google Chrome的核心。这使得 Node.js 的性能非常高。

Node.js应用在单个进程中运行，而无需为每个请求创建新线程。Node.js在其标准库中提供了一组异步 I/O 基元，可防止 JavaScript 代码阻塞，通常 Node.js 中的库是使用非阻塞范例编写的，这使得阻塞行为成为异常而不是常态。

当 Node.js 执行 I/O 操作（如从网络读取、访问数据库或文件系统）时，Node.js 将在响应返回时恢复操作，而不是阻塞线程并浪费 CPU 周期等待。

这使得 Node.js能够处理与单个服务器的数千个并发连接，而不会引入管理线程并发的负担，这可能是 bug 的重要来源。

Node.js具有独特的优势，因为数百万为浏览器编写JavaScript的前端开发人员现在除了编写客户端代码外，还可以编写服务器端代码，而无需学习完全不同的语言。

在 Node.js新的 ECMAScript 标准可以毫无问题地使用，因为您不必等待所有用户更新他们的浏览器 - 您负责通过更改 Node.js版本来决定要使用的 ECMAScript 版本，并且还可以通过运行带有标志的 Node.js来启用特定的实验功能。

## 海量的库

npm以其简单的结构帮助Node.js的生态系统激增，现在npm注册表托管了超过1，000，000个开源软件包，您可以自由使用。

Node.js 最常见的 Hello World 示例是 Web 服务器：

```javascript
const http = require('http')

const hostname = '127.0.0.1'
const port = 3000

const server = http.createServer((req, res) => {
  res.statusCode = 200
  res.setHeader('Content-Type', 'text/plain')
  res.end('Hello World\n')
})

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`)
})
```

此代码首先包含 Node.js [`http` 模块](https://gitee.com/link?target=https%3A%2F%2Fnodejs.org%2Fapi%2Fhttp.html)。

Node.js有一个很棒的[标准库](https://gitee.com/link?target=https%3A%2F%2Fnodejs.org%2Fapi%2F)，包括对网络的一流支持。

的方法创建新的 HTTP 服务器并返回它。`createServer()``http`

服务器设置为侦听指定的端口和主机名。当服务器准备就绪时，调用回调函数，在本例中通知我们服务器正在运行。

每当收到新请求时，都会调用[`请求`事件](https://gitee.com/link?target=https%3A%2F%2Fnodejs.org%2Fapi%2Fhttp.html%23http_event_request)，并提供两个对象：请求（[`http.IncomingMessage`](https://gitee.com/link?target=https%3A%2F%2Fnodejs.org%2Fapi%2Fhttp.html%23http_class_http_incomingmessage) 对象）和响应（[`http.服务器响应`](https://gitee.com/link?target=https%3A%2F%2Fnodejs.org%2Fapi%2Fhttp.html%23http_class_http_serverresponse)对象）。

这 2 个对象对于处理 HTTP 调用至关重要。

第一个提供请求详细信息。在这个简单的示例中，不使用它，但您可以访问请求标头和请求数据。

第二个用于将数据返回给调用方。

在这种情况下，使用：

```javascript
res.statusCode = 200;
```

我们将 statusCode 属性设置为 200，以指示响应成功。

我们设置内容类型标头：

```javascript
res.setHeader('Content-Type', 'text/plain');
```

我们关闭响应，将内容作为参数添加到：`end()`

```javascript
res.end('Hello World\n');
```

## Node.js框架和工具

Node.js是一个低级平台。为了让开发人员变得简单和令人兴奋，社区在Node.js上构建了数千个库。

其中许多随着时间的推移被确立为流行的选择。以下是值得学习的非全面列表：

- [**AdonisJS**](https://gitee.com/link?target=https%3A%2F%2Fadonisjs.com%2F)：一个基于TypeScript的全功能框架，高度关注开发人员的人体工程学，稳定性和信心。Adonis是最快的Node.js Web框架之一。
- [**Egg.js**](https://gitee.com/link?target=https%3A%2F%2Feggjs.org)：使用Node.js和Koa构建更好的企业框架和应用程序的框架。
- [**Express**](https://gitee.com/link?target=https%3A%2F%2Fexpressjs.com%2F)：它提供了创建Web服务器的最简单但最强大的方法之一。它的极简主义方法，不带任何理由，专注于服务器的核心功能，是其成功的关键。
- [**Fastify**](https://gitee.com/link?target=https%3A%2F%2Ffastify.io%2F)：一个高度专注于以最小的开销和强大的插件架构提供最佳开发人员体验的Web框架。Fastify是最快的Node.js Web框架之一。
- [**FeatherJS**](https://gitee.com/link?target=https%3A%2F%2Ffeathersjs.com%2F)：Feathers是一个轻量级的Web框架，用于使用JavaScript或TypeScript创建实时应用程序和REST API。在几分钟内构建原型，在几天内构建生产就绪型应用。
- [**Gatsby**](https://gitee.com/link?target=https%3A%2F%2Fwww.gatsbyjs.com%2F)：一个基于 [React](https://gitee.com/link?target=https%3A%2F%2Freactjs.org%2F) 的、[由 GraphQL](https://gitee.com/link?target=https%3A%2F%2Fgraphql.org%2F) 驱动的静态站点生成器，具有非常丰富的插件和启动器生态系统。
- [**happy**](https://gitee.com/link?target=https%3A%2F%2Fhapi.dev)：用于构建应用程序和服务的丰富框架，使开发人员能够专注于编写可重用的应用程序逻辑，而不是花时间构建基础架构。
- [**koa**](https://gitee.com/link?target=http%3A%2F%2Fkoajs.com%2F)：它是由Express背后的同一团队构建的，旨在以多年的知识为基础，更简单，更小。新项目诞生于在不破坏现有社区的情况下创建不兼容的更改的需要。
- [**Loopback.io**](https://gitee.com/link?target=https%3A%2F%2Floopback.io%2F)：轻松构建需要复杂集成的现代应用程序。
- [**Meteor**](https://gitee.com/link?target=https%3A%2F%2Fmeteor.com)：一个非常强大的全栈框架，通过同构方法使用JavaScript构建应用程序，在客户端和服务器上共享代码，为您提供动力。曾经是一个提供一切的现成工具，现在与前端库[React](https://gitee.com/link?target=https%3A%2F%2Freactjs.org%2F)，[Vue](https://gitee.com/link?target=https%3A%2F%2Fvuejs.org%2F)和[Angular](https://gitee.com/link?target=https%3A%2F%2Fangular.io)集成。也可用于创建移动应用程序。
- [**微**](https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2Fzeit%2Fmicro)：它提供了一个非常轻量级的服务器来创建异步HTTP微服务。
- [**NestJS**](https://gitee.com/link?target=https%3A%2F%2Fnestjs.com%2F)：基于TypeScript的渐进式Node.js框架，用于构建企业级高效，可靠和可扩展的服务器端应用程序。
- [**Next.js**](https://gitee.com/link?target=https%3A%2F%2Fnextjs.org%2F)：[React](https://gitee.com/link?target=https%3A%2F%2Freactjs.org)框架，为您提供最佳的开发人员体验，其中包含生产所需的所有功能：混合静态和服务器渲染，TypeScript支持，智能捆绑，路由预取等。
- [**Nx**](https://gitee.com/link?target=https%3A%2F%2Fnx.dev%2F)：使用NestJS，Express，[React](https://gitee.com/link?target=https%3A%2F%2Freactjs.org%2F)，[Angular](https://gitee.com/link?target=https%3A%2F%2Fangular.io)等进行全栈monorepo开发的工具包！Nx 可帮助您将开发从一个团队构建一个应用程序扩展到多个团队协作开发多个应用程序！
- [**Remix**](https://gitee.com/link?target=https%3A%2F%2Fremix.run)：Remix是一个全栈Web框架，用于为Web构建出色的用户体验。它开箱即用，包含构建现代Web应用程序（前端和后端）并将其部署到任何基于JavaScript的运行时环境（包括Node.js）所需的一切。
- [**Sapper**](https://gitee.com/link?target=https%3A%2F%2Fsapper.svelte.dev%2F)：Sapper是一个用于构建各种规模的Web应用程序的框架，具有漂亮的开发体验和灵活的基于文件系统的路由。提供 SSR 和更多！
- [**Socket.io**](https://gitee.com/link?target=https%3A%2F%2Fsocket.io%2F)：用于构建网络应用程序的实时通信引擎。
- [**Strapi**](https://gitee.com/link?target=https%3A%2F%2Fstrapi.io%2F)：Strapi是一个灵活的开源Headless CMS，使开发人员可以自由选择自己喜欢的工具和框架，同时还允许编辑者轻松管理和分发其内容。通过插件系统使管理面板和API可扩展，Strapi使世界上最大的公司能够加速内容交付，同时构建漂亮的数字体验。