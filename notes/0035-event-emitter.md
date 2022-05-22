# Node.js 事件发射器

如果您在浏览器中使用 JavaScript，您就会知道有多少用户的交互是通过事件处理的：鼠标点击、键盘按钮按下、对鼠标移动做出反应等等。

在后端，Node.js 为我们提供了使用 [`events` 模块](https://nodejs.org/api/events.html) 构建类似系统的选项。

这个模块特别提供了 `EventEmitter` 类，我们将使用它来处理我们的事件。

您使用

```js
const EventEmitter = require('events');
const eventEmitter = new EventEmitter();
```

除了许多其他对象外，该对象还公开了 `on` 和 `emit` 方法。

- `emit` 用于触发事件
- `on` 用于添加一个回调函数，该函数将在事件触发时执行

例如，让我们创建一个 `start` 事件，作为提供示例的问题，我们只需登录到控制台即可对此做出反应：

```js
eventEmitter.on('start', () => {
  console.log('started');
});
```

当我们执行

```js
eventEmitter.emit('start');
```

事件处理函数被触发，我们得到控制台日志。

您可以通过将参数作为附加参数传递给 `emit()` 来将参数传递给事件处理程序：

```js
eventEmitter.on('start', number => {
  console.log(`started ${number}`);
});

eventEmitter.emit('start', 23);
```

多个参数：

```js
eventEmitter.on('start', (start, end) => {
  console.log(`started from ${start} to ${end}`);
});

eventEmitter.emit('start', 1, 100);
```

EventEmitter 对象还公开了其他几种与事件交互的方法，例如

- `once()`: 添加一次性监听器
- `removeListener()` / `off()`：从事件中移除事件监听器
- `removeAllListeners()`：删除事件的所有侦听器

您可以在 https://nodejs.org/api/events.html 的事件模块页面上阅读他们的所有详细信息。