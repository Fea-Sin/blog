## 代码分离

代码分离是 webpack 中最引人注目的特性之一。此特性能够把代码分离到不同的 bundle 中，
然后可以按需加载或并行加载这些文件。代码分离可以用于获取更小的 bundle，以及控制资源
加载优先级，如果使用合理会极大影响加载时间

常用的代码分离方法有三种：

- 入口起点，使用`entry`配置手动地分离代码

- 防止重复，使用`SplitChunksPlugin`去重和分离 chunk

- 动态导入，通过模块中的内联函数调用来分离代码

### 入口起点

这是迄今为止最简单、最直观的分离代码的方式。不过这种方式手动配置较多，并有一些隐患，
我们将会解决这些问题，先看看如何从 main bundle 中分离 another module

```
|- /src
  |- index.js
  |- another-module.js
```

another-module.js

```
import _ from 'lodash';
console.log(
  _.join(['Another', 'module', 'loaded!'])
)
```

webpack.config.js

```
const path = require('path')

module.exports = {
  mode: 'development',
  entry: {
    index: './src/index.js',
    another: './src/another-module.js'
  },
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
}
```

这种方式存在一些隐患

- 如果入口 chunk 之间包含一些重复的模块，那些重复模块都会被引入到各个 bundle 中

- 这种方式不够灵活，并且不能动态地将核心应用程序逻辑中的代码拆分出来

### 防止重复 prevent duplication

`SplitChunksPlugin`插件可以将公共的依赖模块提取到已有的 entry chunk 中，或者提取到
一个新生成的 chunk

### SplitChunksPlugin

`optimization.splitChunks`

the configuration object represents the default behavior of the `SplitChunksPlugin`

```
optimization: {
  splitChunks: {
    chunks: 'async',
    minSize: 30000,
    maxSize: 0,
    minChunks: 1,
    maxAyncRequests: 5,
    maxInitialRequests: 3,
    automaticNameDelimiter: '~',
    name: true,
    cacheGroups: {
      vendors: {
        test: /[\\/]node_modules[\\/]/,
        priority: -10
      },
      default: {
        minChunks: 2,
        priority: -20,
        reuseExistingChunk: true
      }
    }
  }
}
```

`splitChunks.chunks`

This indicates which chunks will be selected for optimization. When a string
is provided, valid values are `all`、`async`、`initial`. Providing `all`can be
particularly powerful, because it means that chunks can be shared even between
async and non-async chunks

```
optimization: {
  splitChunks: {
    chunks: 'all'
  }
}
```

Alternativey you can provide a function

```
optimization: {
  splitChunks: {
    chunks(chunk) {
      // exclude `my-excluded-chunk`
      return chunk.name !== 'my-excluded-chunk'
    }
  }
}
```

使用 `optimization.splitChunks`配置选项，现在可以看到已经从`index.bundle.js`和`another.bundle.js`
中删除了重复的依赖项

一些对于代码分离很有帮助的 plugin 和 loader

- `mini-css-extract-pligin` 用于将 css 从应用程序分离

- `bundle-loader` 用于分离代码和延迟加载生成 bundle

- `promise-loader` 类似于 bundle-loader，但是使用了 promise API

### 动态导入

当涉及到动态代码拆分时，webpack 提供了两个类似的技术。
第一种，也是推荐的方式是，使用符合 ECMAScript 提案的`import()`语法来实现动态导入

```
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    index: './src/index.js'
  },
  output: {
    filename: '[name].bundle.js',
    chunkFilename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
}
```

> 注意，这里使用了 `chunkFilename`，它决定 non-entry-chunk（非入口 chunk）的名称

现在，我们不再使用静态导入`lodash`，而是通过动态导入来分离一个 chunk

src/index.js

```
function getComponent() {
  return import('lodash').then(({default: _}) => {
    var element = document.createElement('div');

    element.innerHTML = _.join('Hello', 'wenpack', ' ');
    return element
  }).catch(error => 'An error occurred while loading  the component')
}

getComponent().then(component => {
  document.body.appendChild(component);
})
```
