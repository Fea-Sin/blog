# Tree-Shaking

tree shaking 是一个术语，通常用于描述移除 JavaScript 上下文中的未引用代码（dead-code）。它依赖
于 ES2015 模块语法的`静态结构`特性，例如`import/export`。这个术语和概念实际上是由 ES2015 模块打包
工具`rollup`普及来的

webpack2 正式版内置支持 ES2015 模块和未使用模块检测能力。新的 webpack4 正式版扩展了此检测能力，通过
`package.json`的`sideEffects`属性作为标记，向 compiler 提供提示，表明项目中的哪些文件是`pure纯正ES2015模块`，
由此可以安全地删除文件中未使用的部分

## 将文件标记为`side-effect-free`（无副作用）

在一个纯粹的 ESM 模块世界中，很容易识别出哪些文件有副作用。然而，我们的项目无法达到这种纯度，
所以，此时有必要提示`webpack compiler`哪些代码是纯粹部分。通过 package.json 的`sideEffects`
属性来实现

如果所有代码都不包含副作用，我们就可以简单地将该属性标记为`false`，来告知 webpack 可以安全地删除
未用到的 export

```json
{
  "name": "your-project",
  "sideEffects": false
}
```

如果你的代码确实有一些副作用，可以改为提供一个数组，此数组支持简单的 glob 模式匹配相关文件，
支持`*`, `**`, `{a,b}`, `[a-z]`。如果匹配模式为`*.css`，且不包含`/`，将被视为`**/*.css`

```json
{
  "name": "your-project",
  "sideEffects": ["./src/some-side-effect-file.js", "*.css"]
}
```

## 结论

- 使用 ES2015 模块语法（即`import`和`export`）

- 确保没有编译器将您的 ES2015 模块语法转为 CommonJS

- 在项目的`package.json`文件中，添加`sideEffects`属性

- 使用`mode`为`production`的配置项以启用更多优化项，包括压缩代码和 tree-shaking
