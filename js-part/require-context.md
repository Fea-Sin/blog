## require-context

```
// https://blog.csdn.net/weixin_45622540/article/details/107505732
```

`require.context`是什么

一个 webpack 的 api，通过执行`require.context`函数获取一个特定的上下文，主要用来
实现自动化导入模块，在前端工程中，如果遇到从一个文件夹引入很多模块的情况，可以使用这个 api，
它会遍历文件夹的指定文件，然后自动导入，使得不需要每次显式的调用 import 导入模块。

`require.context`函数接受三个参数

- directory: `string` 读取文件的路径

- useSubdirectories: `boolean` 是否遍历文件的子目录

- regExp: `RegExp` 匹配文件的正则

实例

```js
const files = require.context("/test", false, /\.test\.js$/);
```

### 返回

上述实例会返回 test 文件夹下的文件的执行环境

`require.context`函数执行后返回的是一个可调用对象（也是就是函数），对象有 3 个属性

- resolve: `function` 接受一个参数 request，request 为 test 文件下匹配文件的相对路径，返回这个
  匹配文件相对于整个工程的相对路径

- keys: `function` keys 函数返回值就是文件下匹配文件的相对路径组成的数组

- id: `string` 执行环境的 id，返回的是一个字符串，主要用在 module.hot.accept（热加载时使用）

同时返回值 files 作为一个函数，也接受一个参数，这个参数与 resolve 所需的参数是一样的，
调用 files 返回的是一个模块，这个模块才是我们真正需要的。

模块内容:

```
files(key).default
```

```
https://stackoverflow.com/questions/54059179/what-is-require-context
```

webpack 编译器的一个主要功能是从入口处递归的解模块，构建所有模块的依赖关系通过分析
`require()`、`require.context()`、`import`和、`import()`表达式。

在 webpack 中关于`context`的解释，跟 Node.js 很相似，通过一些文件夹为基础来解析
一些模块的绝对路径。

使用`require.context`跟 Node.js 在运行时动态构建模块路径是相同的。
`require.context`返回值是一个可调用的对象其功能类似 require，该对象
的 keys 包含模块的数据，并且可以作为参数来获取模块的值。
