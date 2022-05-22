# Node.js events 模块

`events` 模块为我们提供了 EventEmitter 类，这是在 Node.js 中处理事件的关键。

```js
const EventEmitter = require('events');
const door = new EventEmitter();
```

事件侦听器具有以下内置事件：

- 添加监听器时的`newListener`
- 移除监听器时的`removeListener`

以下是最有用的方法的详细说明：

## `emitter.addListener()`

`emitter.on()` 的别名。

## `emitter.emit()`

发出一个事件。 它按照注册的顺序同步调用每个事件监听器。

```js
door.emit('slam'); // emitting the event "slam"
```

## `emitter.eventNames()`

返回一个字符串数组，表示在当前 `EventEmitter` 对象上注册的事件：

```js
door.eventNames();
```

## `emitter.getMaxListeners()`

获取可以添加到 `EventEmitter` 对象的最大侦听器数量，默认为 10，但可以使用 `setMaxListeners()` 增加或减少

```js
door.getMaxListeners();
```

## `emitter.listenerCount()`

获取作为参数传递的事件的侦听器计数：

```js
door.listenerCount('open');
```

## `emitter.listeners()`

获取作为参数传递的事件的侦听器数组：

```js
door.listeners('open');
```

## `emitter.off()`

在 Node.js 10 中添加了 `emitter.removeListener()` 的别名

## `emitter.on()`

添加在发出事件时调用的回调函数。

用法：

```js
door.on('open', () => {
  console.log('Door was opened');
});
```

## `emitter.once()`

添加一个回调函数，在注册 this 后第一次发出事件时调用该函数。 这个回调只会被调用一次，永远不会再被调用。

```js
const EventEmitter = require('events');
const ee = new EventEmitter();
ee.once('my-event', () => {  // call callback function once});
```

## `emitter.prependListener()`

当您使用 `on` 或 `addListener` 添加监听器时，它会在监听器队列中最后添加，并最后调用。 使用 `prependListener` 在其他监听器之前添加和调用它。

## `emitter.prependOnceListener()`

当您使用 `once` 添加监听器时，它会在监听器队列中最后添加，并最后调用。 使用 `prependOnceListener` 在其他监听器之前添加和调用它。

## `emitter.removeAllListeners()`

移除监听特定事件的 `EventEmitter` 对象的所有监听器：

```js
door.removeAllListeners('open');
```

## `emitter.removeListener()`

删除特定的侦听器。 您可以在添加时将回调函数保存到变量中，以便稍后引用它：

```js
const doSomething = () => {};
door.on('open', doSomething);
door.removeListener('open', doSomething);
```

## `emitter.setMaxListeners()`

设置可以添加到 `EventEmitter` 对象的最大侦听器数量，默认为 10，但可以增加或减少。

```js
door.setMaxListeners(50);
```
