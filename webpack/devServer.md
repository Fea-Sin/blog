## devServer

webpack-dev-server 能够用于快速开发应用程序

```
const path = require('path')

module.exports = {
  // ...
  devServer: {
    contentBase: path.join(__dirname, 'dist'),
    compress: true,
    port: 9000
  }
}
```

- devServer.clientLogLevel

`string: 'node', 'info', 'error', 'warning'`

在开发工具的控制台（console）显示消息，默认值是`info`

`devServer.clientLogLevel`可能会显得很繁琐，可以通过将其设置为`none`来关闭 log

```
module.exports = {
  // ...
  devServer: {
    clientLogLevel: 'none'
  }
}
```

CLI 用法
`webpack-dev-server --client-log-level none`

- devServer.compress

`boolean`

一切服务都启用`gzip`压缩

- devServer.contentBase

`boolean: false` `string` `[string]` `number`

告诉服务器从哪个目录中提供内容，只有在你想要提供静态文件时才需要

`devServer.publicPath`将用于确定应该从哪里提供 bundle，并且此选项优先

> 推荐使用一个绝对路径

```
module.exports = {
  devServer: {
    contentBase: path.join(__dirname, 'public')
  }
}
```

也可以从多个目录提供内容

```
module.exports = {
  devServer: {
    contentBase: [path.join(__dirname, 'public'), path.join(__dirname, 'assets')]
  }
}
```

CLI 用法
`webpack-dev-server --content-base /path/to/content/dir`

- devServer.headers

`object`

在所有响应中添加首部内容

```
devServer: {
  headers: {
    'X-Custom-Foo': 'bar'
  }
}
```

- devServer.historyApiFallback

`boolean` `object`

当使用 HTML5 History API 时，任意的`404`响应都可能需要被替换为`index.html`

`devServer.historyApiFallback`默认禁用，通过传入以下启用

```
devServer: {
  historyApiFallback: true
}
```

通过传入一个对象，比如使用`rewrites`这个选项，进一步控制

```
devServer: {
  historyApiFallback: {
    rewrites: [
      { from: /^\/$/, to: '/views/landing.html'},
      { from: /^\/subpage/, to: '/views/subpage.html' },
      { from: /./, to: '/views/404.html' }
    ]
  }
}
```

- devServer.host

`string`

指定使用一个 host，默认是`localhost`。如果你希望服务器外部可访问，指定如下

```
devServer: {
  host: '0.0.0.0'
}
```

CLI 用法
`webpack-dev-server --host 0.0.0.0`

- devServer.hot

`boolean`
启用 webpack 的模块热替换功能

```
devServer: {
  hot: true
}
```

> 注意，必须有`webpack.HotModuleReplacementPlugin`才能完全开启 HMR。
> 如果`webpack`或`webpack-dev-server`是通过`--hot`选项启动的，那么这个插件会被自动添加，
> 所以你可能不需要把它添加到`webpack.config.js`中

- devServer.inline

`boolean`

在 dev-server 的两种不同模式之间切换。默认情况下，应用程序启用内联模式(inline mode)。
这意味着一段处理实时重载的脚本插入到你的包(bundle)中，并且构建消息将会出现在浏览器控制台

> 推荐使用模块热替换的内联模式，因为它包含来自 webpack 的 HMR 触发器

- devServer.lazy

`boolean`

当启用`devServer.lazy`时，dev-server 只有在请求时才编译包(bundle)。这意味着 webpack 不会监视任何
文件改动，我们称之为**懒惰模式**

```
devServer: {
  lazy: true
}
```

- devServer.noInfo

`boolean`

告诉 dev-server 隐藏 webpack bundle 信息之类的消息。`devServer.noInfo`默认禁用

```
devServer: {
  noInfo: true
}
```

- devServer.open

`boolean` `string`

告诉 dev-server 在 server 启动后打开浏览。默认禁用

```
devServer: {
  open: true
}
// or
devServer: {
  open: 'Google Chrome'
}
```

- devServer.openPage

`string`
指定打开浏览器时的导航页面

```
devServer: {
  openPage: '/different/page'
}
```

- devServer.overlay

`boolean` `object: { boolean errors, boolean warnings }`

当出现编译错误或警告时，在浏览器中显示全屏覆盖层。默认禁用，如果你只想显示编译错误：

```
devServer: {
  overlay: true
}
```

显示警告和错误

```
devServer: {
  warnings: true,
  errors: true
}
```

- devServer.port

`number`

指定要监听请求的端口号

```
devServer: {
  port: 8080
}
```

CLI 用法
`webpack-dev-server --port 8080`
