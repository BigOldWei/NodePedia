# Node.js简介

Node.js 是一个开源和跨平台的 JavaScript 运行时环境。它是几乎任何类型的项目的流行工具！

Node.js 在浏览器之外运行 Google Chrome 的核心 V8 JavaScript 引擎。这使得 Node.js 非常高效。

Node.js 应用程序在单个进程中运行，无需为每个请求创建新线程。 Node.js 在其标准库中提供了一组异步 I/O 原语，可防止 JavaScript 代码阻塞，通常，Node.js 中的库是使用非阻塞范例编写的，使阻塞行为成为例外而不是常态。

当 Node.js 执行 I/O 操作时，例如从网络读取、访问数据库或文件系统，而不是阻塞线程并浪费 CPU 周期等待，Node.js 将在响应返回时恢复操作。

这允许 Node.js 处理与单个服务器的数千个并发连接，而不会引入管理线程并发的负担，这可能是错误的重要来源。

Node.js 具有独特的优势，因为数百万为浏览器编写 JavaScript 的前端开发人员现在能够在编写客户端代码的同时编写服务器端代码，而无需学习完全不同的语言。

在 Node.js 中，可以毫无问题地使用新的 ECMAScript 标准，因为您不必等待所有用户更新他们的浏览器 - 您可以通过更改 Node.js 版本来决定使用哪个 ECMAScript 版本，您还可以通过运行带有标志的 Node.js 来启用特定的实验功能。

## 大量的库

npm 以其简单的结构帮助 Node.js 的生态系统激增，现在 npm 注册表托管了超过 1,000,000 个开源包，您可以免费使用。

## 一个示例 Node.js 应用程序

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

此代码首先包含 Node.js [`http` 模块](https://nodejs.org/api/http.html)。

Node.js 有一个很棒的 [标准库](https://nodejs.org/api/)，包括一流的网络支持。

`http` 的 `createServer()` 方法创建一个新的 HTTP 服务器并返回它。

服务器设置为侦听指定的端口和主机名。当服务器准备好时，回调函数被调用，在这种情况下通知我们服务器正在运行。

每当收到新请求时，都会调用 [`request` 事件](https://nodejs.org/api/http.html#http_event_request)，提供两个对象：一个请求（一个 [`http.IncomingMessage`]( https://nodejs.org/api/http.html#http_class_http_incomingmessage) 对象）和响应（一个 [`http.ServerResponse`](https://nodejs.org/api/http.html#http_class_http_serverresponse) 对象）。

这两个对象对于处理 HTTP 调用至关重要。

第一个提供请求详细信息。在这个简单的示例中，没有使用它，但您可以访问请求标头和请求数据。

第二个用于将数据返回给调用者。

在这种情况下：

```js
res.statusCode = 200;
```

we set the statusCode property to 200, to indicate a successful response.

We set the Content-Type header:

```js
res.setHeader('Content-Type', 'text/plain');
```

and we close the response, adding the content as an argument to `end()`:

```js
res.end('Hello World\n');
```

## Node.js 框架和工具

Node.js 是一个低级平台。为了使开发人员的工作变得简单和令人兴奋，社区在 Node.js 上构建了数千个库。

随着时间的推移，其中许多被确立为流行的选择。以下是值得学习的非全面列表：

- [**AdonisJS**](https://adonisjs.com/)：一个基于 TypeScript 的全功能框架，高度关注开发人员的人体工程学、稳定性和信心。 Adonis 是最快的 Node.js Web 框架之一。
- [**Egg.js**](https://eggjs.org/)：使用 Node.js 和 Koa 构建更好的企业框架和应用程序的框架。
- [**Express**](https://expressjs.com/)：它提供了一种创建 Web 服务器的最简单但功能强大的方法。其极简主义的方法，没有意见，专注于服务器的核心功能，是其成功的关键。
- [**Fastify**](https://fastify.io/)：一个高度专注于以最少的开销和强大的插件架构提供最佳开发人员体验的 Web 框架。 Fastify 是最快的 Node.js Web 框架之一。
- [**FeatherJS**](https://feathersjs.com/)：Feathers 是一个轻量级的 Web 框架，用于使用 JavaScript 或 TypeScript 创建实时应用程序和 REST API。在几分钟内构建原型，在几天内构建生产就绪的应用程序。
- [**Gatsby**](https://www.gatsbyjs.com/)：基于 [React](https://reactjs.org/)，[GraphQL](https://graphql.org/ ) 驱动的静态站点生成器，具有非常丰富的插件和启动器生态系统。
- [**hapi**](https://hapi.dev/)：用于构建应用程序和服务的丰富框架，使开发人员能够专注于编写可重用的应用程序逻辑，而不是花时间构建基础设施。
- [**koa**](http://koajs.com/)：它由 Express 背后的同一团队构建，旨在更简单、更小，建立在多年的知识之上。新项目的诞生是为了在不破坏现有社区的情况下创建不兼容的更改。
- [**Loopback.io**](https://loopback.io/)：使构建需要复杂集成的现代应用程序变得容易。
- [**Meteor**](https://meteor.com/)：一个非常强大的全栈框架，为您提供同构的方法来使用 JavaScript 构建应用程序，在客户端和服务器上共享代码。曾经是提供一切的现成工具，现在与前端库 [React](https://reactjs.org/)、[Vue](https://vuejs.org/) 和 [Angular]( https://angular.io/)。也可用于创建移动应用程序。
- [**Micro**](https://github.com/zeit/micro)：它提供了一个非常轻量级的服务器来创建异步 HTTP 微服务。
- [**NestJS**](https://nestjs.com/)：基于 TypeScript 的渐进式 Node.js 框架，用于构建企业级高效、可靠和可扩展的服务器端应用程序。
- [**Next.js**](https://nextjs.org/)：[React](https://reactjs.org/) 框架，为您提供最佳的开发人员体验以及生产所需的所有功能：混合静态和服务器渲染、TypeScript 支持、智能捆绑、路由预取等。
- [**Nx**](https://nx.dev/)：使用 NestJS、Express、[React](https://reactjs.org/)、[Angular]( https://angular.io/) 等等！ Nx 有助于将您的开发从一个团队构建一个应用程序扩展到多个团队协作开发多个应用程序！
- [**Remix**](https://remix.run/)：Remix 是一个全栈 Web 框架，用于为 Web 构建出色的用户体验。它开箱即用，包含构建现代 Web 应用程序（前端和后端）并将它们部署到任何基于 JavaScript 的运行时环境（包括 Node.js）所需的一切。
- [**Sapper**](https://sapper.svelte.dev/)：Sapper 是一个用于构建各种规模的 Web 应用程序的框架，具有优美的开发体验和灵活的基于文件系统的路由。提供 SSR 等等！
- [**Socket.io**](https://socket.io/)：构建网络应用的实时通信引擎。
- [**Strapi**](https://strapi.io/)：Strapi 是一个灵活的开源无头 CMS，让开发人员可以自由选择自己喜欢的工具和框架，同时还允许编辑者轻松管理和分发他们的内容。通过插件系统使管理面板和 API 可扩展，Strapi 使世界上最大的公司能够加速内容交付，同时构建美妙的数字体验。