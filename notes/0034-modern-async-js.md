# 带有 Async 和 Await 的现代异步 JavaScript

## 介绍

JavaScript 在很短的时间内从回调演变为 Promise（ES2015），并且由于 ES2017 异步 JavaScript 使用 async/await 语法变得更加简单。

异步函数是 Promise 和生成器的组合，基本上，它们是对 Promise 的更高级别的抽象。让我再说一遍：**async/await 是建立在 Promise 之上的**。

## 为什么要引入 async/await？

它们减少了围绕 Promise 的样板，以及链式 Promise 的“不破坏链”限制。

在 ES2015 中引入 Promise 时，它们旨在解决异步代码的问题，他们确实做到了，但是在 ES2015 和 ES2017 分离的 2 年中，很明显*promises 不可能是最终的解决方案*。

引入 Promise 是为了解决著名的 *callback hell* 问题，但它们本身就引入了复杂性和语法复杂性。

它们是很好的原语，可以向开发人员提供更好的语法，所以在合适的时候我们得到了**异步函数**。

它们使代码看起来像是同步的，但在幕后它是异步且非阻塞的。

## 这个怎么运作

一个异步函数返回一个 Promise，如下例所示：

```js
const doSomethingAsync = () => {
  return new Promise(resolve => {
    setTimeout(() => resolve('I did something'), 3000);
  });
};
```

当你想**调用**这个函数时，你会在前面加上`await`，并且**调用代码将停止，直到promise被解决或拒绝**。 一个警告：客户端函数必须定义为“异步”。 这是一个例子：

```js
const doSomething = async () => {
  console.log(await doSomethingAsync());
};
```

## 一个简单的例子

这是一个用于异步运行函数的 async/await 的简单示例：

```javascript
const doSomethingAsync = () => {
  return new Promise(resolve => {
    setTimeout(() => resolve('I did something'), 3000)
  })
}

const doSomething = async () => {
  console.log(await doSomethingAsync())
}

console.log('Before')
doSomething()
console.log('After')
```

## 承诺一切

将 `async` 关键字添加到任何函数意味着该函数将返回一个承诺。

即使它没有明确地这样做，它也会在内部使它返回一个承诺。

这就是此代码有效的原因：

```js
const aFunction = async () => {  return 'test';};
aFunction().then(alert); // This will alert 'test'
```

它与以下内容相同：

```js
const aFunction = () => {  return Promise.resolve('test');};
aFunction().then(alert); // This will alert 'test'
```

## 代码更容易阅读

正如您在上面的示例中看到的，我们的代码看起来非常简单。 将其与使用普通 Promise 的代码进行比较，带有链接和回调函数。

这是一个非常简单的例子，当代码复杂得多时，主要的好处就会出现。

例如，您将如何获取 JSON 资源并使用 Promise 对其进行解析：

```js
const getFirstUserData = () => {
  return fetch('/users.json') // get users list
    .then(response => response.json()) // parse JSON
    .then(users => users[0]) // pick first user
    .then(user => fetch(`/users/${user.name}`)) // get user data
    .then(userResponse => userResponse.json()); // parse JSON
};

getFirstUserData();
```

这是使用 await/async 提供的相同功能：

```js
const getFirstUserData = async () => {
  const response = await fetch('/users.json'); // get users list
  const users = await response.json(); // parse JSON
  const user = users[0]; // pick first user
  const userResponse = await fetch(`/users/${user.name}`); // get user data
  const userData = await userResponse.json(); // parse JSON
  return userData;
};

getFirstUserData();
```

## 多个异步函数串联

异步函数可以很容易地链接起来，并且语法比普通的 Promise 更具可读性：

```javascript
const promiseToDoSomething = () => {
  return new Promise(resolve => {
    setTimeout(() => resolve('I did something'), 10000)
  })
}

const watchOverSomeoneDoingSomething = async () => {
  const something = await promiseToDoSomething()
  return something + '\nand I watched'
}

const watchOverSomeoneWatchingSomeoneDoingSomething = async () => {
  const something = await watchOverSomeoneDoingSomething()
  return something + '\nand I watched as well'
}

watchOverSomeoneWatchingSomeoneDoingSomething().then(res => {
  console.log(res)
})
```

## 更容易调试

调试 Promise 很困难，因为调试器不会跳过异步代码。

Async/await 使这变得非常容易，因为对于编译器来说它就像同步代码一样。

