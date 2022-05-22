# 使用 exports 从 Node.js 文件公开功能

Node.js 有一个内置的模块系统。

Node.js 文件可以导入其他 Node.js 文件公开的功能。

当你想导入你使用的东西时

```js
const library = require('./library');
```

导入驻留在当前文件夹中的 `library.js` 文件中公开的功能。

在此文件中，必须先公开功能，然后才能被其他文件导入。

默认情况下，文件中定义的任何其他对象或变量都是私有的，不会暴露给外部世界。

这就是 [`module` 系统](https://nodejs.org/api/modules.html) 提供的 `module.exports` API 允许我们做的事情。

当您将对象或函数分配为新的 `exports` 属性时，这就是被公开的东西，因此，它可以导入到应用程序的其他部分或其他应用程序中。

您可以通过 2 种方式做到这一点。

第一个是将一个对象分配给`module.exports`，这是一个由模块系统开箱即用的对象，这将使您的文件导出*只是那个对象*：

```js
// car.js
const car = {
  brand: 'Ford',
  model: 'Fiesta',
};

module.exports = car;
```

第二种方法是将导出的对象添加为 `exports` 的属性。 这种方式允许您导出多个对象、函数或数据：

```js
const car = {
  brand: 'Ford',
  model: 'Fiesta',
};

exports.car = car;
```

或者直接：

```js
exports.car = {
  brand: 'Ford',
  model: 'Fiesta',
};
```

在另一个文件中，您将通过引用导入的属性来使用它：

```js
const items = require('./items');
const { car } = items;
```

或者您可以使用解构赋值：

```js
const { car } = require('./items');
```

`module.exports` 和 `exports` 有什么区别？

第一个公开它指向的对象。 后者暴露了它指向的对象的*属性*。

`require` 将始终返回 `module.exports` 指向的对象。

```js
// car.js
exports.car = {
  brand: 'Ford',
  model: 'Fiesta',
};

module.exports = {
  brand: 'Tesla',
  model: 'Model S',
};

// app.js
const tesla = require('./car');
const ford = require('./car').car;

console.log(tesla, ford);
```

这将打印 `{ brand: 'Tesla', model: 'Model S' } undefined`，因为 `require` 函数的返回值已更新为 `module.exports` 指向的对象，所以 *the property* that ` 无法访问添加的exports`。