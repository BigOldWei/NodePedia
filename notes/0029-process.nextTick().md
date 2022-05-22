# 了解 process.nextTick()

当您尝试理解 Node.js 事件循环时，其中一个重要部分是 `process.nextTick()`。

每次事件循环完成一次完整的行程时，我们称之为一次滴答。

当我们将函数传递给 `process.nextTick()` 时，我们指示引擎在当前操作结束时调用此函数，在下一个事件循环滴答开始之前：

```js
process.nextTick(() => {
  // do something
});
```
事件循环正忙于处理当前函数代码。

当此操作结束时，JS 引擎会运行在该操作期间传递给 `nextTick` 调用的所有函数。

这是我们可以告诉 JS 引擎异步处理函数（在当前函数之后）的方式，但要尽快，而不是排队。

调用 `setTimeout(() => {}, 0)` 将在下一个时钟周期结束时执行函数，比使用 `nextTick()` 优先级调用并在下一个时钟周期开始之前执行它时要晚得多 .

当你想确保在下一个事件循环迭代中代码已经被执行时，使用`nextTick()`。
