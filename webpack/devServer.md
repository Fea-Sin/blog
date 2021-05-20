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

- # devServer.clientLogLevel

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

- # devServer.compress

`boolean`

一切服务都启用`gzip`压缩

- # devServer.contentBase

`boolean: false` `string` `[string]` `number`

告诉服务器从哪个目录中提供内容，只有在你想要提供静态文件时才需要

`devServer.publicPath`将用于确定应该从哪里提供 bundle，并且此选项优先

> 推荐使用一个绝对路径

默认情况下，将使用当前工作目录作为提供内容的目录。将其设置为`false`以禁用`contentBase`

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

- # devServer.headers

`object`

在所有响应中添加首部内容

```
devServer: {
  headers: {
    'X-Custom-Foo': 'bar'
  }
}
```

- # devServer.historyApiFallback

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

- # devServer.host

`string`

指定使用一个 host，默认是`localhost`。如果你希望服务器外部可访问，指定如下

```
devServer: {
  host: '0.0.0.0'
}
```

CLI 用法
`webpack-dev-server --host 0.0.0.0`

- # devServer.hot

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

- # devServer.inline

`boolean`

在 dev-server 的两种不同模式之间切换。默认情况下，应用程序启用内联模式(inline mode)。
这意味着一段处理实时重载的脚本插入到你的包(bundle)中，并且构建消息将会出现在浏览器控制台

> 推荐使用模块热替换的内联模式，因为它包含来自 webpack 的 HMR 触发器

- # devServer.lazy

`boolean`

当启用`devServer.lazy`时，dev-server 只有在请求时才编译包(bundle)。这意味着 webpack 不会监视任何
文件改动，我们称之为**懒惰模式**

```
devServer: {
  lazy: true
}
```

- # devServer.noInfo

`boolean`

告诉 dev-server 隐藏 webpack bundle 信息之类的消息。`devServer.noInfo`默认禁用

```
devServer: {
  noInfo: true
}
```

- # devServer.open

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

- # devServer.overlay

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

- # devServer.port

`number`

指定要监听请求的端口号

```
devServer: {
  port: 8080
}
```

CLI 用法
`webpack-dev-server --port 8080`

- # devServer.proxy

`object` `object, function`

如果你有单独的后端开发服务器 API，并且希望在同域名下发送 API 请求，那么代理某些 URL 会很有用

dev-server 使用了非常强大的 http-proxy-middleware 包。

在`localhost:3000`上有后端服务的话，你可以这样启用代理

```
module.exports = {
  devServer: {
    proxy: {
      '/api': 'http://localhost:3000'
    }
  }
}
```

请求到`/api/users`现在会被代理到请求`http://localhost:3000/api/users`

如果你不想始终传递`/api`，则需要这样写

```
devServer: {
  proxy: {
    '/api': {
      target: 'http://localhost:3000',
      pathRewrite: {'^api': ''}
    }
  }
}
```

此时，请求`/api/users`会被代理到`http://localhost:3000/users`

默认情况下，不接受运行在 HTTPS 上，且使用了无效证书的后端服务器。如果你想要接受，修改配置如下

```
devServer: {
  proxy: {
    '/api': {
      target: 'https://other-server.example.com',
      secure: false
    }
  }
}
```

有时你不想代理所有的请求，可以基于一个函数的返回值绕过代理

在函数中你可以访问请求体、响应体和代理选项。必须返回`false`或路径，来跳过代理请求。

例如，对于浏览器请求，你想要提供一个 HTML 页面，但是对于 API 请求则保持代理

```
devServer: {
  proxy: {
    '/api': {
      target: 'http://localhost:3000',
      bypass: function(req, res, proxyOptions) {
        if (req.headers.accept.indexOf('html') !== -1) {
          console.log('Skipping proxy for browser request')
          return '/index.html'
        }
      }
    }
  }
}
```

如果你想要代理多个路径特定到同一个 target 下，你可以使用一个或多个具有`context`属性的对象构成的数组

```
devServer: {
  proxy: [{
    context: ['/auth', '/api'],
    target: 'http://localhost:3000'
  }]
}
```

注意，默认情况下，根请求不会被代理。要启用根代理，应该将`devServer.index`选项指定为 falsy 值

```
devServer: {
  index: '', // specify to enable root proxy
  host: '...',
  contentBase: '...',
  proxy: {
    context: () => true,
    target: ''http://localhost:1234'
  }
}
```

The origin of the host header is kept when proxying by default, you can set `changeOrigin` to `true`
to override this behaviour. It is useful in some cases like using name-based virtual hosted sites.

```
devServer: {
  proxy: {
    '/api': 'http://localhost:3000',
    changeOrigin: true
  }
}
```

- # devServer.progress 只用于命令行工具(CLI)

`boolean`

将运行进度输出到控制台

```
webpack-dev-server --progress
```

- # devServer.public

`string`

当使用内联模式(inliine mode)并代理`dev-server`时，内联的客户端脚本并不总是知道要连接到什么地方。
它会尝试根据`window.location`来猜测服务器的 URL，但是如果失败，你需要使用这个配置

例如，`dev-server`被代理到 nginx，并且在`myapp.test`上可用

```
devServer: {
  public: 'myapp.test:80'
}
```

CLI 用法

```
webpack-dev-server --public myapp.test:80
```

- # devServer.publicPath

`string`

此路径下的打包文件可在浏览器中访问

假设服务器运行在`http://localhost:8080`并且`output.filename`被设置为`bundle.js`。
默认`devServer.publicPath`是`/`，所以你的包(bundle)可以通过`http://localhost:8080/bundle.js`访问

修改`devServer.publicPath`，将 bundle 放在指定目录下

```
devServer: {
  publicPath: '/assets/'
}
```

现在可以通过`http://localhost:8080/assets/bundle.js`访问 bundle

> 确保`devServer.publicPath`总是以`/`开头和结尾

也可以使用一个完整的 URL，这是模块热替换所比需的

```
devServer: {
  publicPath: 'http://localhost:8080/assets/'
}
```

可以通过`http://localhost:8080/assets/bundle.js`访问 bundle

> devServer.publicPath 和 output.publicPath 一样被推荐

- # devServer.quiet

`boolean`

启用`devServer.quiet`后，除了初始启动信息之外的任何内容都不会被打印到控制台。这意味着来自 webpack 的错误或警告
在控制台不可见

```
devServer: {
  quiet: true
}
```

CLI 用法 `webpack-dev-server --quiet`

- # devServer.socket

`string`

用于监听的 Unix socket（而不是 host）

```
devServer: {
  socket: 'socket'
}
```

CLI 用法 `webpack-dev-server --socket socket`

- # devServer.staticOptions

可以用于对`contentBase`路径下提供的静态文件，进行高级选项配置。

```
devServer: {
  staticOptions: {
    redirect: false
  }
}
```

> 这只有使用`devServer.contentBase`是一个`string`时才有效

- # devServer.stats

`string: 'none' | 'errors-only' | 'minimal' | 'normal' | 'verbose'` `object`

通过此选项，可以精确控制要显示的 bundle 信息。如果你想要显示一些打包信息，但又不是显示全部，这可能
是一个不错的妥协

想要在 bundle 中只显示错误

```
devServer: {
  stats: 'errors-only'
}
```

> 此选项在配置 `quiet`或`noInfo`时无效

- # devServer.watchContentBase

告知 dev-server，server 服务`devServer.contentBase`选项下的文件，开启此选项后，在文件
修改之后，会触发一次完整的页面重载

```
devServer: {
  watchContentBase: true
}
```

CLI 用法`webpack-dev-server --watch-content-base`

- # devServer.watchOptions

与监控文件相关的控制选项

- # devServer.writeToDisk

`boolean: false` `function(filePath)`

Tells `devServer` to write generated assets to the disk

```
devServer: {
  writeToDisk: true
}
```

Providing a Function to devServer.writeToDisk can be used for filtering.
The function follows the same premise as `Array#filter` in which a boolean
return value tells if the file should be written to disk

```
devServer: {
  writeToDisk: (filePaht) => {
    return /superman\.css$/.test(filePath)
  }
}
```
