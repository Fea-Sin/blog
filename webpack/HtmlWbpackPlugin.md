## HtmlWebpackPlugin

`HtmlWebpackPlugin`简化了 HTML 文件的创建，这对于那些文件名中包含哈希值，并且哈希值会随着每次编译而改变的 webpack 包
特别有用。你可以让该插件为你生成一个 HTML 文件，使用`lodash模版`提供模版

### 基本用法

该插件将为你生成一个 HTML5 文件，在 body 中使用`script`标签引入你所有 webpack 生成的 bundle，
只需添加该插件到你的 webpack 配置中。

如果你有多个 webpack 入口，他们都会在已生成 HTML 文件中用`script`标签引入

如果在 webpack 的输出中有任何 CSS 资源（例如，使用`MinCssExtractPlugin提起的CSS`），那么这些资源
也会在 HTML 文件`<head></head>`元素用`<link>`标签引入
