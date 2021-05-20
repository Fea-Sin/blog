## 解析 resolve

这些选项能设置模块如何被解析，webpack 提供合理的默认值，但是还是可能会修改一些细节。

- # resolve

`object`
配置模块如何解析。当在 ES2015 中调用`import 'lodash'`，`resolve`选项能够对 webpack 查找`lodash`
的方式去做修改

- # resolve.alias

`object`

创建`import`或`require`的别名，来确保模块引入变得更简单。例如，一些位于`src/`文件夹下的常用模块

```
module.exports = {
  resolve: {
    alias: {
      Utilities: path.resolve(__dirname, 'src/utilities'),
      Templates: path.resolve(__dirname, 'src/templates')
    }
  }
}
```

现在，替换在导入时使用相对路径这种方式，就像这样

```
import Utilities from '../../utilities/utility'
```

你可以这样使用别名

```
import Utility from 'Utilities/utility'
```

也可以在给定对象的键后的末尾添加`$`，以表示精准匹配

```
resolve: {
  alias: {
    xyz$: path.resolve(__dirname, 'path/to/file.js')
  }
}
```

这将产生以下结果

```
import Test1 from 'xyz' //精确匹配，所以path/to/file.js被解析和导入
import Test2 from 'xyz/file.js'  // 非精确匹配，触发普通解析
```

- # resovle.enforceExtension

`boolean: false`

如果是`true`，将不允许无扩展名（extension-less）文件

如果`./foo`默认有`.js`扩展，`require(./foo)`可以正常运行，但如果设置为`true`，
只有`require('./foo.js')`才能正常工作

`enforceExtension`默认是`false`

- # resolve.extensions

`[string]`: `['.wasm', '.mjs', '.js', '.json']`

自动解析确定的扩展。默认值为

```
resolve: {
  extensions: ['.wasm', '.mjs', '.js', '.json']
}
```

能够使用户在引入模块时不带扩展

```
import File from '../path/to/file'
```

> 使用此选项，会覆盖默认数组，这就意味着 webpack 将不再尝试使用默认扩展来解析模块。
> 对于使用其扩展导入的模块，例如，`import SomeFile from './somefile.ext'`
> 想要正确解析，一个包含`'*'`的字符串必须包含在数组中

- resolve.mainFields

`[string]`

当从 npm 包中导入模块时（例如`import * as D3 from 'd3'`），此选项将决定在`package.json`中使用
哪个字段导入模块。根据 webpack 配置中指定的`target`不同，默认值也会有所不同。

当`target`属性设置为`webworker`，`web`或者没有指定，默认值为：

```
resolve: {
  mainFields: ['browser', 'module', 'main']
}
```

对于其它任意的`target`(包括 node)，默认值为：

```
resolve: {
  mainFields: ['module', 'main']
}
```

例如，考虑任意一个名为`upstream`的 library，其`package.json`包含以下字段

```
{
  "browser": "build/upstream.js",
  "module": "index"
}
```

在我们`import * as Upstream from 'upstream'`时，这实际上会从`browser`属性解析文件。在这里
`browser`属性时最优选择的，因为它时`mainFields`的第一项。

同时，有 webpack 打包的 Node.js 应用程序首先尝试从`module`字段解析文件

- # resolve.mainFiles

`[string]: ['index']`

解析目录时要使用的文件名

```
resolve: {
  mainFiles: ['index']
}
```

- # resolveLoader

`object`

这组选项与上面的`resolve`对象属性集合相同，但仅用于解析 webpack 的 loader 包。默认：

```
resolveLoader: {
  modules: ['node_modules'],
  extensions: ['.js', '.json'],
  mainFields: ['loader', 'main']
}
```
