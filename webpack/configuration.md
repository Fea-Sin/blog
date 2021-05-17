## 配置 configuration

webpack 配置文件，导出 webpack 配置对象的 JavaScript 文件，然后 webpack 会根据此配置对象上定义的属性
进行处理

不应当编写很长的配置，应该将配置拆分成多个文件

## webpack 模块

在模块化编程中，开发者将程序分解为功能离散的 chunk，并称之为模块

每个模块具有比完整程序更小的接触面，使得验证、调试、测试轻而易举。精心编写的模块提供了可靠的抽象
和封装界限，使得应用程序中每个模块，都具有条理清楚的设计和明确的目的

Node.js 从最开始就支持模块化编程。而然，web 的模块化支持正缓慢到来。在 web 存在多种支持 JavaScript 模块化
的工具，这些工具各有优势和限制。webpack 基于从这些系统获得的经验教训，并将模块的概念用于项目中的任何文件

于 Node.js 模块相比，webpack 模块能够以各种方式表达它们的依赖关系

- ES2015 `import`语句

- CommonJS `require()`语句

- AMD `define`和`require`语句

- css/sass/less 问价中的`@import`语句

- 样式`url(...)`或 HTML 文件`<img src="..." />`中的图片链接

> webpack1 需要特定的 loader 来转换 ES2015 `import`，然而，webpack2 这一切都是开箱即用的

### 支持的模块类型

通过 loader，webpack 可以支持以各种语言和预处理语法编写的模块。loader 描述了 webpack 如何处理
非 JavaScript 模块，并且在 bundle 中引入这些依赖。webpack 社区已经为各种流行语言和语言处理器
创建了 loader 包括

- CoffeeScript

- TypeScript

- ESNext(Babel)

- Sass

- Less

- Stylus

还有很多，总的来所，webpack 提供了可定制的、强大的和丰富的 API，允许在任何技术栈中使用 webpack，
同时在开发、测试和生产环境下的工作流程中做到**无侵入性**
