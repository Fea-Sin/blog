## watch

启用 watch 模式，这意味着在初始构建之后，webpack 将继续监听任何已解析文件的更改

webpack.config.js

```
module.exports = {
  // ...
  watch: true
}
```

> webpack-dev-server 和 webpack-dev-middleware 里 watch 默认开启

### watchOptions

一组用来定制 watch 模式的选项
webpack.config.js

```
module.exports = {
  // ...
  watchOptions: {
    aggregateTimeout: 300,
    poll: 1000
  }
}
```

- aggregateTimeout

  `number: 300`
  当一个文件更改，会在重新构建前增加延迟，这个选项允许 webpack 将这段时间内进行的任何其它更改
  都聚合到一次重新构建里。以毫秒为单位

- watchOptions.ignored

`RegExp` `anymath`

对于某些系统，监听大量文件系统会导致大量的 CPU 和内存占用。这个选项可以排除一些巨大的文件夹，
例如：`node_modules`

```
module.exports = {
  watchOptions: {
    ignored: /node_modules/
  }
}
```

也可以使用多种 anymatch 模式

```
watchOptions: {
  ignored: ['files/**/*.js', 'node_modules']
}
```

- watchOptions.poll

`boolean: false` `number`

指定毫秒为单位进行轮询

```
watchOptions: {
  poll: 1000 // 每秒检查一次变动
}
```

- info-verbosity

`string: 'none', 'info', 'verbose'`

控制声明周期消息的详细程度，例如`Started watching files(开始监听文件)`日志。
将`info-verbosity`设置为`verbose`，还会额外在增量构建的开始和结束时，向控制台
发送消息。`info-verbosity`默认设置为`info`

```
webpack --watch --info-verbosity verbose
```
