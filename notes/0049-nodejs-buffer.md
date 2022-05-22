# Node.js 缓冲区

## 什么是缓冲区？

缓冲区是内存区域。与使用每天直接与内存交互的系统编程语言（如 C、C++ 或 Go）的程序员相比，大多数 JavaScript 开发人员不太熟悉这个概念。

它表示在 V8 JavaScript 引擎之外分配的固定大小的内存块（无法调整大小）。

您可以将缓冲区想象成一个整数数组，每个整数代表一个数据字节。

它由 Node.js [Buffer 类](https://nodejs.org/api/buffer.html) 实现。

## 为什么我们需要缓冲区？

引入缓冲区是为了帮助开发人员在传统上只处理字符串而不是二进制文件的生态系统中处理二进制数据。

Node.js 中的缓冲区与缓冲数据的概念无关。当流处理器接收数据的速度超过其消化速度时，就会发生这种情况。

##如何创建缓冲区

使用 [`Buffer.from()`](https://nodejs.org/api/buffer.html#buffer_buffer_from_buffer_alloc_and_buffer_allocunsafe)、[`Buffer.alloc()`](https://nodejs.org) 创建缓冲区/api/buffer.html#buffer_class_method_buffer_alloc_size_fill_encoding) 和 [`Buffer.allocUnsafe()`](https://nodejs.org/api/buffer.html#buffer_class_method_buffer_allocunsafe_size) 方法。

```js
const buf = Buffer.from('Hey!');
```

- [`Buffer.from(array)`](https://nodejs.org/api/buffer.html#buffer_class_method_buffer_from_array)
- [`Buffer.from(arrayBuffer[, byteOffset[, length\]])`](https://nodejs.org/api/buffer.html#buffer_class_method_buffer_from_arraybuffer_byteoffset_length)
- [`Buffer.from(buffer)`](https://nodejs.org/api/buffer.html#buffer_class_method_buffer_from_buffer)
- [`Buffer.from(string[, encoding\])`](https://nodejs.org/api/buffer.html#buffer_class_method_buffer_from_string_encoding)

您也可以只初始化传递大小的缓冲区。 这将创建一个 1KB 缓冲区：

```js
const buf = Buffer.alloc(1024);
```

或

```js
const buf = Buffer.allocUnsafe(1024);
```

虽然 `alloc` 和 `allocUnsafe` 都分配了一个以字节为单位的指定大小的 `Buffer`，但由 `alloc` 创建的 `Buffer` 将用零*初始化*，而由 `allocUnsafe` 创建的缓冲区将*未初始化* . 这意味着虽然 `allocUnsafe` 与 `alloc` 相比会相当快，但分配的内存段可能包含可能敏感的旧数据。

较旧的数据（如果存在于内存中）可以在读取“缓冲区”内存时被访问或泄漏。 这才是真正使 `allocUnsafe` 不安全的原因，使用它时必须格外小心。

## 使用缓冲区

### 访问缓冲区的内容

缓冲区是一个字节数组，可以像数组一样访问：

```js
const buf = Buffer.from('Hey!');
console.log(buf[0]); // 72
console.log(buf[1]); // 101
console.log(buf[2]); // 121
```

这些数字是标识缓冲区中字符的 UTF-8 字节（`H` → `72`，`e` → `101`，`y` → `121`）。 这是因为 `Buffer.from()` 默认使用 UTF-8。 请记住，某些字符可能在缓冲区中占用超过一个字节（`é` → `195 169`）。

您可以使用 `toString()` 方法打印缓冲区的全部内容：

```js
console.log(buf.toString());
```

`buf.toString()` 默认也使用 UTF-8。

> 请注意，如果您使用设置其大小的数字初始化缓冲区，您将可以访问包含随机数据的预初始化内存，而不是空缓冲区！

### 获取缓冲区的长度

使用 `length` 属性：

```js
const buf = Buffer.from('Hey!');
console.log(buf.length);
```

### 遍历缓冲区的内容

```js
const buf = Buffer.from('Hey!');
for (const item of buf) {
  console.log(item); // 72 101 121 33
}
```

### 更改缓冲区的内容

您可以使用 `write()` 方法将整个数据字符串写入缓冲区：

```js
const buf = Buffer.alloc(4);
buf.write('Hey!');
```

就像您可以使用数组语法访问缓冲区一样，您也可以以相同的方式设置缓冲区的内容：

```js
const buf = Buffer.from('Hey!');
buf[1] = 111; // o in UTF-8
console.log(buf.toString()); // Hoy!
```

### 切片缓冲区

如果要创建缓冲区的部分可视化，可以创建切片。 切片不是副本：原始缓冲区仍然是事实的来源。 如果这种情况发生变化，您的切片就会发生变化。

使用 `subarray()` 方法来创建它。 第一个参数是起始位置，您可以指定一个可选的第二个参数和结束位置：

```js
const buf = Buffer.from('Hey!');
buf.subarray(0).toString(); // Hey!
const slice = buf.subarray(0, 2);
console.log(slice.toString()); // He
buf[1] = 111; // o
console.log(slice.toString()); // Ho
```

### 复制一个缓冲区

使用 `set()` 方法可以复制缓冲区：

```js
const buf = Buffer.from('Hey!');
const bufcopy = Buffer.alloc(4); // allocate 4 bytes
bufcopy.set(buf);
```

默认情况下，您复制整个缓冲区。 如果您只想复制缓冲区的一部分，您可以使用 `.subarray()` 和指定要写入的偏移量的 `offset` 参数：

```js
const buf = Buffer.from('Hey?');
const bufcopy = Buffer.from('Moo!');
bufcopy.set(buf.subarray(1, 3), 1);
bufcopy.toString(); // 'Mey!'
```
