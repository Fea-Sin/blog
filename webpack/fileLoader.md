## file-loader

`file-loader`解析文件中的`import/require()`为 url，并触发文件到输出文件夹

例如在 bundle 文件中引入一个目标文件

```js
// file.js
import img from "./images/hello.png";
```

在 webpack 中增加配置

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.(png|jpe?g|gif)$/i,
        use: [
          {
            loader: "file-loader",
          },
        ],
      },
    ],
  },
};
```

> 默认情况下，结果文件名是文件内容的哈希值，加上原始文件的扩展名

### Options

**name**

类型： `string|function`
默认： `[contenthash].[ext]`

- string

```js
rules: [
  {
    test: /\.(png|jpe?g|gif)$/i,
    loader: "file-loader",
    options: {
      name: "[path][name].[ext]",
    },
  },
];
```

- function

```js
options: {
  name(resourcePath, resourceQuery) {
    // `resourcePath`: `/absolute/path/to/file.js`
    // `resourceQuery`: `?foo=bar`
    if (process.env.NODE_ENV === 'development') {
      return '[path][name].[ext]'
    }
    return '[contenthash].[ext]'
  }
}
```

> 默认情况，指定 path 和 name，文件会输出为当前文件夹，并使用
> 相同的路径获取文件

**outputPath**

类型： `string|function`
默认： `undefined`

- string

```js
rules: [
  {
    test: /\.(png|jpe?g|gif)$/i,
    loader: "file-loader",
    options: {
      name: "[contenthash].[ext]",
      outputPath: "images",
    },
  },
];
```

- function

```js
options: {
  outputPath: (url, resourcePath, context) => {
    // `resourcePath` is original absolute path to asset
    // `context` is directory where stored asset(`rootContext`) or `context` option
    // 获取相对路径
    // const relativePath = path.relative(context, resourcePath)

    if (/my-custom-image\.png/.test(resourcePath)) {
      return `other_public_path/${url}`;
    }
    if (/images/.test(context)) {
      return `image_out_path/${url}`;
    }
    return `public_path/${url}`;
  };
}
```

**publicPath**

类型：`string|function`
默认：`__webpack_public_path__`+outputPath

- string

```js
rules: [
  {
    test: /\.(png|jpe?g|gif)$/i,
    loader: "file-loader",
    options: {
      name: "[contenthash].[ext]",
      outputPath: "images",
      publicPath: "assets",
    },
  },
];
```

- function

```js
options: {
  publicPath: (url, resourcePath, context) => {
    if (/my-custom-image\.png/.test(resourcePath)) {
      return `other_public_path/${url}`;
    }
    if (/images/.test(context))) {
      return `image_output_path/${url}`
    }
    return `public_path/${url}`
  };
}
```

**context**

类型：`string`
默认：项目中配置的 context 选项

**emitFile**

类型：`boolean`
默认：`true`

如果是`true` emits a file(写文件到文件系统)
如果是`false`loader 会返回 public URL will not emit the file
这个选项，在服务端包使用`false`

**regExp**

类型：`RegExp`
默认：`undefined`

**esModule**

类型：`boolean`
默认：`true`

默认情况，`file-loader`使用 ES modules 语法生成 JS modules，在一些场景下
使用 ES modules 是有好处的，例如 `tree shaking`

```js
rules: [
  {
    test: /\.css$/,
    use: [
      {
        loader: "file-loader",
        options: {
          esModule: false,
        },
      },
    ],
  },
];
```
