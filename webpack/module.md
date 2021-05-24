## 模块 module

这些选项决定了如何处理项目中的不同类型的模块

- # module.noParse

`RegExp` `[RegExp]` `function(resource)` `string` `[string]`

防止 webpack 解析那些于给定正则表达式相匹配的文件。忽略的文件中不应该含有
`import` `require` `define`的调用，或任何其它导入机制。忽略大型的
library 可以提高构建性能

```
module.exports = {
  module: {
    noParse: /jquery|lodash/,
  }
}
```

```
module: {
  noParse: (content) => /jquery|lodash/.test(content)
}
```

- # module.rules

`[Rule]`

创建模块时，匹配请求的规则数组。这些规则能够修改模块的创建方式，这些规则能够
对模块应用 loader 或者修改解析器(parse)

- # Rule

`object`

每个规则可以分为三部分条件（condition）、结果（result）和嵌套规则（nested rule）

- # Rule 条件

条件有两种输入值

1 resource: 请求文件的绝对路径，它已经根据 resolve 规则解析

2 issuer: 被请求资源的模块文件的绝对路径。
例如，从`app.js`导入`./style.css`，resource 是`path/to/style.css`
issuer 是`/path/to/app.js`

- # 嵌套的 Rule

可以使用属性`rules`和`oneOf`指定嵌套规则

这些规则用于在规则条件(rule condition)匹配时进行取值

- # Rule.enforce

`string`

可能的值有：`'pre' | 'post'`，指定 loader 种类，没有值表示是普通 loader

还有一个额外的种类`行内loader`loader 被应用在`import/require`行内

- # Rule.exclude

`Rule.exclude`是`Rule.resource.exclude`的简写。如果提供了`Rule.exclude`
选项，就不能再提供`Rule.resource`

- # Rule.include

`Rule.include`是`Rule.resource.include`的简写。如果提供了`Rule.include`
就不能再提供`Rule.resource`

- # Rule.loader

`Rule.loader`是`Rule.use: [ { loader } ]` 的简写

- # Rule.oneOf

规则数组，当规则匹配时，只使用第一个匹配规则

```
module.exports = {
  module: {
    test: /\.css$/,
    oneOf: [
      {
        resourceQuery: /inline/, // foo.css?inline,
        use: 'url-loader'
      },
      {
        resourceQuery: /external/, // foo.css?external
        use: 'file-loader'
      }
    ]
  }
}
```

- # Rule.parser

解析选项对象

```
module: {
  rules: [
    // ...
    parse: {
      amd: false, // 禁用 AMD
      commonjs: false, // 禁用 CommonJS
      system: false, // 禁用 SystemJS
      harmony: false, // 禁用 ES2015 Harmony import/export
      requireInclude: false, // 禁用 require.include
      requireEnsure: false, // 禁用 require.ensure
      requireContext: false, //禁用 require.context
      browserify: false, // 禁用 browserify bundle
      requireJs: false, // 禁用 requirejs.*
      node: false, // 禁用 __diranme, __filename, module, require.extensions, require.main等
      node: {...} // 在模块级别上重新配置node 层
    }
  ]
}
```

- # Rule.test

`Rule.test`是`Rule.resource.test`的简写。如果你提供了一个`Rule.test`选项，就不能再提供`Rule.resource`

- # UseEntry

`object` `function(info)`

必须有一个`loader`属性是字符串。它使用 loader 解析选项(resolveLoader)，相对于配置中的`context`来解析

可以有一个`options`属性为字符串或对象，值可以传递到 loader 中，将其理解为 loader 选项

```
module: {
  rules: [
    loader: 'css-loader',
    options: {
      module: true
    }
  ]
}
```

- # Rule.use

`[UseEntry]` `function(info)`

`Rule.use`可以是一个应用于模块的 UseEntries 数组，每个入口指定使用一个 loader

传递字符串，如`use: ['style-loader']`是 loader 属性的简写方式 `use: [{ loader: 'style-loader' }]`

Loaders can be chained by passsing multiple loaders, which will be applied from
right to left(last to first configured)

```
module: {
  rules: [
    {
      // ...
      use: [
        'style-loader',
        {
          loader: 'css-loader',
          options: {
            importLoaders: 1
          }
        },
        {
          loader: 'less-loader',
          options: {
            noIeCompat: true
          }
        }
      ]
    }
  ]
}
```

函数方法

`Rule.use` can also be a function which receives the object argument decribing
the module being loaded and must return an array of UseEntry items

the `info`object parameter has the following fileds

- `compiler` The current webpack compiler (can be undefined)

- `issuer` The path to the module that is importing the module being loaded

- `realResource` Always the path to the module being loaded

- `resource` The path to the module being loaded, it is usually equally to `realResource` except
  when the resource name is overwritten via `!=!`in request string

```
module: {
  rules: [
    {
      use: (info) => ([
        {
          loader: 'custom-svg-loader'
        },
        {
          loader: 'svg-loader',
          options: {
            plugins: [{
              cleanupIDs: { prefix: basename(info.resource) }
            }]
          }
        }
      ])
    }
  ]
}
```
