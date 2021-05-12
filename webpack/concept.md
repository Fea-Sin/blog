## 概念

本质上，webpack 是一个现代 JavaScript 应用程序的静态模块打包工具。当 webpack 处理应用程序时，
它会在内部构建一个依赖图，此依赖图会映射项目所需的每个模块，并生成一个或多个 bundle

从 `v4.0.0`开始，webpack 可以不用再引入一个配置文件来打包项目，然而它仍然有着高度配置性

在开始之前需要先理解一些核心概念

- 入口(entry)

- 输出(output)

- loader

- 插件(plugin)

- 模式(mode)

- 浏览器兼容性(browser compatibility)

### 入口(entry)

入口指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始

默认值是`./src/index.js`

### 输出(output)

output 属性告诉 webapck 在哪里输出它所创建的 bundle，以及如何命名这些文件。

主要输出文件的默认值是`./dist/main.js`，其它生成文件默认放置在`./dist`文件夹中

### loader

webpack 只能理解 JavaScript 和 JSON 文件，loader 让 webpack 能够去处理其它类型的文件，
并将他们转换为有效模块，以供应用程序使用和被添加到依赖图中

> loader 能够让`import`导入任何类型的模块（例如`.css`文件），
> 这是 webpack 特有功能，这种语言扩展是很有必要的
> 这可以创建出更准确的依赖关系图

在 webpack 的配置中 loader 有两个属性

- `test`属性，用于标识出应该被对应的 loader 进行转换的某个或某些文件

- `use`属性，表示使用哪个 loader

`test`和`use`告诉 webpack 编译器(compiler)如下信息

> webpack 编译器，当你碰到`require/import`语句中被解析为`.txt`的路径时，在打包
> 之前先用`raw-loader`转换一下

> 使用正则表达式匹配文件时，不要为它添加引号。
> `/\.txt$/`与`'/\txt$/'`不一样，前者指示 webpack 匹配任何以`.txt`结尾的文件
> 后者指示 webpack 匹配具有绝对路径`'.txt'`的单个文件

### 插件(plugin)

loader 用于转换某些类型的模块，而插件则可以用于执行范围更广泛的任务。包括：打包优化、
资源管理、注入环境变量

插件接口功能极其强大，可以用来处理各种各样的任务

想要使用一个插件，你只需要`require`它，然后把它添加到`plugins`数组中。多数
插件可以通过选项(option)自定义。你也可以在一个配置文件中因不同目的而多次使用同一个插件，
这时需要通过使用`new`操作符来创建它的一个实例。

示例中`html-webpack-plugin`为应用程序生成一个 HTML 文件，并自动注入所有
生成的 bundle

### webpack.config.js

```
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack') // 用于访问内置插件

module.exports = {
  entry: './path/to/entry.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
}
```

### 模式(mode)

通过选择`development`、`production`和`none`之中的一个，来设置`mode`参数，可以启用
webpack 内置在相应环境下的优化

其默认值为`production`

### 浏览器兼容性

webpack 支持所有符合 ES5 的浏览器（不支持 IE8 及以下版本），webpack 的`import`和`require.ensure()`
需要`Promise`，如果想要支持旧版本浏览器，在使用表达式之前，还需要提前加载 polyfill
