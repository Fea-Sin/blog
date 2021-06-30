## MiniCssExtractPlugin

这插件可以抽离 CSS 到单独文件，它为每个 JS 文件包含的 CSS 创建 CSS 文件，它支持
CSS 按需加载和 SourceMaps
它需要 webpack 4，依赖 webpack 4 特性（module types）

安装

```
npm install --save-dev mini-css-extract-plugin
```

建议将`mini-css-extract-plugin`和`css-loader`一起使用

### 如何使用

`mini-css-extract-plugin`更多使用在`production`模式，将 css 分离成单独文件，
对于`development`模式(包括`webpack-dev-server`)你可以使用`style-loader`，
因为原本它是使用多个`<style></style>`将 CSS 插入到 DOM，开发模式会更快

> 注意
> `style-loader`和`mini-css-extract-plugin`不能共用

```js
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const devMode = process.env.NODE_ENV !== "production";

const plugins = [];
if (devMode) {
  plugins.push(new MiniCssExtraxtPlugin());
}

module.exports = {
  plugins,
  module: {
    rules: [
      {
        test: /\.(sa|sc|c)ss$/,
        use: [
          devMode ? "style-loader" : MiniCssExtractPlugin.loader,
          "css-loader",
          "postcss-loader",
          "sass-loader",
        ],
      },
    ],
  },
};
```

### Plugin Options

**filename**
类型： `string|function`
默认：`[name].css`

确定每个输出 CSS 文件的名称，works like `output.filename`

**chunkFilename**
类型： `string|function`
默认： `based on filename`

> `chunkFilename` 只能在 webpack 5 才可以用 `function`

### Loader Options

**publicPath**

类型： `string|function`
默认： the`publicPath`in`webpackOptions.output`

指定一个自己的 public path 像其它的资源一样如 images 等

- string

```js
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
module.exports = {
  plugins: [
    new MiniCssExtractPlugin({
      filename: "[name].css",
      chunkFilename: "[id].css",
    }),
  ],
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              plulicPath: "/public/path/to/",
            },
          },
        ],
      },
    ],
  },
};
```
