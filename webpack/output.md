## 输出 output

`output` 位于对象最顶级键(key)，包括了一组选项，指示 webpack 如何去输出、以及在哪里输出你的 bundle、assets
和其它你所打包或使用 webpack 载入的任何内容

- # output.chunkFilename

`string`

此选项决定了非入口(non-entry)chunk 文件的名称。注意，这些文件名称需要在 runtime 根据 chunk 发送的请求去生成。
因此，需要在 webpack.runtime 输出 bundle 值时，将 chunk id 的值对应映射到占位符(如`name`和`chunkhash`)。

默认使用`[id].js`或从`output.filename`中推断出的值(`name`会被预先替换为`[id]`或`[id].`)。

- # output.crossOriginLoading

`boolean` `string`

只用于`target`是 web，使用了通过 script 标签的 JSONP 来按需加载 chunk

启用 cross-origin 属性加载 chunk。以下是可接收的值

`crossOriginLoading: false` 禁用跨域加载（默认）

`crossOriginLoading: 'anonymous'` 不带凭据(credential)启用跨域加载

`crossOriginLoading: 'use-credentials'` 带凭据启用跨域加载 with credential

- # output.jsonpScriptType

`string`

允许自定义`script`的类型，webpack 会将`script`标签注入到 DOM 中以下载异步 chunk，可以使用以下选型

`text/javascript`默认

`module`与 ES6 就绪代码一起使用

- # output.filename

`string` `function`
此选项决定了每个输出 bundle 的名称。这些 bundle 将写入到`output.path`选项指定的目录下。

对于单个入口起点，filename 会是一个静态名称

```
module.exports = {
  output: {
    filename: 'bundle.js'
  }
}
```

然而，当通过多个入口起点(entry point)、代码拆分(code splitting)或各种插件(plugin)创建多个 bundle，
应该使用以下一种替换方式，来赋予每个 bundle 一个唯一的名称

**使用入口名称**

```
output: {
  filename: '[name].bundle.js'
}
```

**使用内部 chunk id**

```
output: {
  filename: '[id].bundle.js'
}
```

**使用每次构建过程中，唯一的 hash 生成**

```
output: {
  filename: '[name].[hash].bundle.js'
}
```

**使用基于每个 chunk 内容的 hash**

```
output: {
  filename: '[chunkhash].bundle.js'
}
```

Using hashes generated for extracted content

```
output: {
  filename: '[contenthash].bundle.css'
}
```

**使用函数生成 filename**

```
output: {
  filename: (chunkData) => {
    return chunkData.chunk.name === 'main' ? '[name].js' :  '[name]/[name].js'
  }
}
```

注意此选项被称为文件名，但是你可以使用像`'js/[name]/bundle.js'`这样的文件夹结构

注意，此选项不会影响那些`按需加载chunk`的输出文件。对于这些文件，请使用`output.chunkFilename`
选项来控制输出。通过 loader 创建的文件也不受影响，这种情况下，你必须尝试 loader 特定的可用选项

可以使用以下替换模版字符串(通过 webpack 内部的`TemplatedPathPlugin`)

| 模版        | 描述                                            |
| ----------- | ----------------------------------------------- |
| [hash]      | 模块标识符(module identifier)的 hash            |
| [chunkhash] | chunk 内容的 hash                               |
| [name]      | 模块名称                                        |
| [id]        | 模块标识符(module identifier)                   |
| [query]     | 模块的 query，例如文件名`?`后面的字符串         |
| [function]  | the function, which can return filename`string` |

`[hash]` 和 `[chunkhash]`的长度可以使用 `[hash:16]`(默认为 20)。或者，
通过指定`output.hashDigestLength`在全局配置长度。

如果将这个选项设为一个函数，函数将返回一个包含上面表格中替换信息的对象

> 在使用`ExtractTextWebpackPlugin`时，可以用`[contenthash]`来获取提取
> 文件的 hash（既不是[hash]也不是[chunkhash]）

- # output.hashDigest

再生成 hash 时使用的编码方式，默认为`hex`。支持 Node.js`hash.digest`的所有编码。
读文件名使用`base64`可能会出现问题，因为 base64 字母表中具有`/`这个字符(character)。
同样`latin1`规定可以含有任何字符(character)

- # output.hashDigestLength

散列摘要的前缀长度，默认是 20

- # output.jsonpFunction

`string`

只在`target`是 web 时使用，用于按需加载(load on-demand)chunk 的 JSONP 函数
JSONP 函数用于异步加载(async load)chunk，或者拼接多个初始 chunk(SplitChunksPlugin,
AggressiveSplittingPlugin)。

如果在同一网页中使用了多个(来自不同编译过程 compilation 的)webpack runtime，则需要
修改此选项。

如果使用了`output.library`选项，library 名称自动追加的

- # output.library

`string`或`object`(从 webpack3.1.0 开始，用于`libraryTarget: 'umd'`)

`output.library`的值的作用，取决于`output.libraryTarget`选项的值。

注意，`output.libraryTarget`的默认选项是`var`。

如果使用以下配置选项

```
output: {
  library: 'MyLibrary'
}
```

如果生成的输出文件，是在 HTML 页面中作为一个 script 标签引入，则变量将与入口文件
的返回值绑定。

> 注意，如果将数组作为`entry`，那么只会暴露数组中的最后一个模块
> 如果将对象作为`entry`，还可以使用`array`语法暴露

- # output.libraryTarget

`string: 'var'`

配置如何暴露 library。可以使用下面的选项中的任意一个。
注意，此选项与分配给`output.library`的值一同使用

**暴露为一个变量**

`libraryTarget: 'var'` 默认值，当 library 加载完成，入口起点的返回值
将分配给一个变量

```
var MyLibrary = _entry_return_;

// 在一个单独的script
MyLibrary.doSomething();
```

> 当使用此选项时，将`output.library`设置为空，会因为没有变量
> 导致无法赋值

`libraryTarget: 'assign'` 这将产生一个隐含的全局变量，可能会潜在地重新
分配到全局已经存在的值（谨慎使用）

**通过在对象上赋值暴露**

这些选项将入口起点的返回值，赋值给一个特定对象的属性（此名称由`output.library`定义）

如果`output.library`未赋值为一个非空字符串，则默认行为是，将入口起点返回的
所有属性都赋值给一个对象（此对象由`output.libraryTarget`特定）

`libraryTarget: 'this'` 入口起点的返回值将分配给 this 的一个属性（此名称由
`library`定义）

```
this['MyLibrary'] = _entry_return_

// 在一个单独的script
this.MyLibrary.doSomething();
MyLibrary.doSomething(); // 如果 this 是 window
```

`libraryTarget: 'window'` 入口起点的返回值将使用`output.library`中定义值，
分配给`window`对象的这个属性下

```
window['MyLibrary'] = _entry_return_;
```

`libraryTarget: 'global'` 入口起点的返回值将使用`output.library`中定义的值，
分配给`global`对象下

```
global['MyLibrary'] = _entry_return_;
global.MyLibrary.dosomething();
```

`libraryTarget: 'commonjs'` 入口起点的返回值将使用`output.library`中定义的值，
分配给 exports 对象。这个名称也意味着，模块用于 CommonJS 环境

```
exports['MyLibrary'] = _entry_return_;

require('MyLibrary').doSomething();
```

**模块定义系统**

这些选项将导致 bundle 带有更完整的模块头部，以确保与各种模块的兼容。

根据`output.libraryTarget`选项不同，`output.library`选项将具有不同的含义

`libraryTarget: 'commonjs2'` 入口起点的返回值将分配给`module.exports`对象。
这个名称也意味着模块用于 CommonJS 环境

```
module.exports = _entry_return_;
require('MyLibrary').doSomething();
```

注意`output.library`会被省略，因此对于特定的`output.linbraryTarget`，无需再
设置`output.library`

> commonjs 和 commonjs2 很相似，但二者之间存在一些微妙的差异
> 这通常与 webpack 上下文没有关联

`libraryTarget: 'amd'` 将你的 library 暴露为 AMD 模块

AMD 模块要求入口 chunk（例如使用 script 标签加载的第一个脚本）通过特定的属性定义。
例如`define`和`require`，它们通常由 RequireJS 或任何兼容的模块加载器提供。否则直接加载生成的
AMD bundle 将导致报错

```
output: {
  library: 'MyLibrary',
  libraryTarget: 'amd'
}
```

生成的 output 将会使用'MyLibrary'作为模块名定义

```
define('MyLibrary', [], function() {
  return _entry_return_;
})
```

可以在 script 标签中，将 bundle 作为一个模块整体引入，并且可以像这样调用 bundle

```
require(['MyLibrary'], function(MyLibrary) {
  // do something
})
```

如果直接加载`script`标签，此 bundle 无法按照预期运行，或者根本无法正常运行，只能通过
文件的实际路径，在 RequireJS 兼容的异步模块加载器中运行。因此在这种情况下，如果这些设置
直接暴露在服务器上，那么`output.path`和`output.filename`对这个特定的设置可能变得很重要

`libraryTarget: 'umd'` 将你的 library 暴露为所有的模块定义下都可运行的方式。它将在
CommonJS，AMD 环境下运行，或将模块导出到 global 下的变量

```
output: {
  library: 'MyLibrary',
  libraryTarget: 'umd'
}
```

最终输出

```
(function webpackUniversalModuleDefinition(root, factory) {
  if (typeof exports === 'object' && typeof module === 'object')
    module.exports = factory();
  else if (typeof define === 'function' && define.amd)
    define([], factory)
  else if (typeof exports === 'object')
    exports['MyLibrary'] = factory();
  else
    root['MyLibrary'] = factory();
})(typeof self !== 'undefined' ? self : this, function() {
  return _entry_return_;
})
```

从 webpack 3.1.0 开始，你可以将`library`指定一个对象，用于给每个 target 起不同的名称

```
output: {
  library: {
    root: 'MyLibrary',
    amd: 'my-library',
    commonjs: 'my-common-library'
  },
  libraryTarget: 'umd'
}
```

`libraryTarget: 'jsonp'`
这将把入口起点的返回值，包裹到一个 jsonp 包装容器中

- # output.path

`string`
output 目录对应一个绝对路径

```
output: {
  path: path.resolve(__dirname, 'dist/assets')
}
```

- # output.pathinfo

`boolean`

告知 webpack 在 bundle 中引入所有包含模块信息的相关注释。此选项在`development`模式时默认值是`true`，
而在`production`模式时默认值是`false`

> 在开发环境(development)下注释可以提供非常有用的数据信息

```
output: {
  pathinfo: true
}
```

注意，这些注释会被添加至经过 tree shaking 后生成的 bundle 中

- # output.publicPath

`string` `function`

对于按需加载（on-demand-load）或加载外部资源（external resources）（如图片、文件等）来说，output.publicPath
是很重要的选项。如果指定了一个错误的值，加载这些资源时就会收到 404 错误

此选项指定在浏览器中所引用的此输出目录对应的公开 URL。相对 URL 会被相对于 HTML 页面（或 base 标签）解析。相对于
服务的 URL，相对于协议的 URL 或绝对 URL 也是可能用到的，或者有时必须用到的，例如：当将资源托管到 CDN 时。

该选项的值是以 runtime(运行时)或 loader(载入时)所创建的每个 URL 的前缀。因此，在多数情况下，此选项的值都会
以`/`结束

简单规则如下: `output.path`中的 URL 以 HTML 页面为基准

```
output: {
  path: path.resolve(__dirname, 'public/assets'),
  publicPath: 'https://cdn.example.com/assets/'
}
```

下面的配置

```
module.exports = {
  output: {
    publicPath: '/assets/',
    chunkFilename: '[id].chunk.js'
  }
}
```

对于一个 chunk 请求，看起来像这样`/assets/4.chunk.js`
对于一个输出 HTML 的 loader 可能会这样输出
`<link href='/assets/spinner.gif' />`
或者在加载 CSS 的一个图片时
`background-image: url(/assets/spinner.gif)`

webpack-dev-server 也会默认从`publicPath`为基准，使用它来决定在哪个目录下启用服务，来访问 webpack
输出的文件。

> 注意，参数中的`[hash]`将会被替换为编译过程(compilation)的 hash

```
output: {
  // one of the below
  publicPath: 'https://cdn.example.com/assets', // CDN
  publicPath: '//cdn.example.com/assets',
  publicPath: '/assets/' // 相对于服务(server-relative)
  publicPath: 'assets/' // 相对于HTML页面
  publicPath: '../assets/' // 相对于HTML页面
  publicPath: '', //相对于HTML页面（目录相同）
}
```

在编译时无法知道输出文件的`publicPath`的情况下，可以留空，然后在入口
文件（entry file）处使用自由变量`__webpack_public_path__`，以便在运行时进行动态设置

```
__webpack_public_path__ = myRuntimePublicPath;
```

- # output.sourceMapFilename

`string`
此选项会向硬盘写入一个文件，只在`devtool`启用了 SourceMap 选项时才使用

配置 source map 的命名方式，默认使用'[file].map'

`file`占位符会被替换为原始文件的文件名
