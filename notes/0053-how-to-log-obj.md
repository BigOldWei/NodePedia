# 如何在 Node.js 中打印对象

当您在浏览器中运行的 JavaScript 程序中键入 console.log() 时，这将在浏览器控制台中创建一个不错的条目：

[![console log browser](http://img.weidawang.site/i/2022/05/22/6289f5c480eb8.png)](https://nodejs.dev/static/2a5f076a9531f84a0e8e655923a55d1c/410f3/console-log-browser.png)

单击箭头后，日志会展开，您可以清楚地看到对象属性：

[![console log browser expanded](http://img.weidawang.site/i/2022/05/22/6289f5c4a1f66.png)](https://nodejs.dev/static/176d7e6c1b9c0c0d48626733a70dace6/0b5b1/console-log-browser-expanded.png)

在 Node.js 中，同样的情况也会发生。

当我们在控制台上记录一些东西时，我们并没有那么奢侈，因为如果你手动运行 Node.js 程序，这会将对象输出到 shell，或者输出到日志文件。 您将获得对象的字符串表示形式。

现在，一切都很好，直到一定程度的嵌套。 经过两层嵌套后，Node.js 放弃并打印 `[Object]` 作为占位符：

```js
const obj = {
  name: 'joe',
  age: 35,
  person1: {
    name: 'Tony',
    age: 50,
    person2: {
      name: 'Albert',
      age: 21,
      person3: {
        name: 'Peter',
        age: 23,
      },
    },
  },
};
console.log(obj);
```

```console
{
  name: 'joe',
  age: 35,
  person1: {
    name: 'Tony',
    age: 50,
    person2: {
      name: 'Albert',
      age: 21,
      person3: [Object]
    }
  }
}
```

如何打印整个对象？

这样做的最好方法，同时保留漂亮的打印，是使用

```js
console.log(JSON.stringify(obj, null, 2));
```

其中“2”是用于缩进的空格数。

另一种选择是使用

```js
require('util').inspect.defaultOptions.depth = null;
console.log(obj);
```

但问题是 2 级之后的嵌套对象现在被展平了，这可能是复杂对象的问题。

如果你不想接触任何类型的 `defaultOptions`，一个完美的选择是 `console.dir`。

```js
// `depth` tells util.inspect() how many times to recurse while formatting the object, default is 2
console.dir(obj, {
  depth: 10,
});

// ...or pass `null` to recurse indefinitely
console.dir(obj, {
  depth: null,
});
```

