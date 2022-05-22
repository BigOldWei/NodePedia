# Node.js 与 WebAssembly

WebAssembly 是一种高性能的类汇编语言，可以从包括 C/C++、Rust 和 AssemblyScript 在内的多种语言编译。 截至目前，Chrome、Firefox、Safari、Edge 和 Node.js 都支持它！

WebAssembly 规范详细说明了两种文件格式，一种称为 WebAssembly 模块的二进制格式，扩展名为 .wasm，对应的文本表示称为 WebAssembly 文本格式，扩展名为 .wat。

## 关键概念

- 模块 - 已编译的 WebAssembly 二进制文件，即 `.wasm` 文件。
- 内存 - 可调整大小的 ArrayBuffer。
- 表 - 不存储在内存中的可调整大小的类型化引用数组。
- 实例 - 具有内存、表和变量的模块的实例化。

为了使用 WebAssembly，你需要一个 `.wasm` 二进制文件和一组 API 来与 WebAssembly 通信。 Node.js 通过全局 `WebAssembly` 对象提供必要的 API。

```js
console.log(WebAssembly);
/*
Object [WebAssembly] {
  compile: [Function: compile],
  validate: [Function: validate],
  instantiate: [Function: instantiate]
}
*/
```

## 生成 WebAssembly 模块

有多种方法可用于生成 WebAssembly 二进制文件，包括：

- 手工编写 WebAssembly (`.wat`) 并使用 [wabt](https://github.com/webassembly/wabt) 等工具转换为二进制格式
- 在 C/C++ 应用程序中使用 [emscripten](https://emscripten.org/)
- 将 [wasm-pack](https://rustwasm.github.io/wasm-pack/book/) 与 Rust 应用程序一起使用
- 如果您喜欢类似 TypeScript 的体验，请使用 [AssemblyScript](https://www.assemblyscript.org/)

> 其中一些工具不仅会生成二进制文件，还会生成 JavaScript “胶水”代码和相应的 HTML 文件以在浏览器中运行。

＃＃ 如何使用它

一旦你有了一个 WebAssembly 模块，你就可以使用 Node.js 的 WebAssembly 对象来实例化它。

```js
// Assume add.wasm file exists that contains a single function adding 2 provided arguments
const fs = require('fs');

const wasmBuffer = fs.readFileSync('/path/to/add.wasm');
WebAssembly.instantiate(wasmBuffer).then(wasmModule => {
  // Exported function live under instance.exports
  const { add } = wasmModule.instance.exports;
  const sum = add(5, 6);
  console.log(sum); // Outputs: 11
});
```

## 与操作系统交互

WebAssembly 模块无法自行直接访问操作系统功能。 第三方工具 [Wasmtime](https://docs.wasmtime.dev/) 可用于访问此功能。 `Wasmtime` 利用 [WASI](https://wasi.dev/) API 来访问操作系统功能。

## 资源

- [一般 WebAssembly 信息](https://webassembly.org/)
- [MDN 文档](https://developer.mozilla.org/en-US/docs/WebAssembly)
- [手动编写 WebAssembly](https://webassembly.github.io/spec/core/text/index.html)



