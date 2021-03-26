## require 与 import 的区别

### 起源

require 和 import 是不同模块化规范下的引入模块的语句。

es6 诞生之前，js 一直没有模块系统。就连 css 都有`@import`，对复杂的大型项目
来说无疑是一种阻碍。

社区给出的模块化的方案，是 node.js 提供的模块化也叫`CommonJS`

> node.js 由[Ryan Dahl](https://en.wikipedia.org/wiki/Ryan_Dahl)，[其 github](https://github.com/ry)
>
> 在 2009 年发布，node.js 主要用于服务端开发

CommonJS 模块化方案 require/exports 是为服务端开发设计的。服务器模块系统同步读取模块
文件的内容，编译执行后得到模块的接口（node.js 是 CommonJS 规范的实现）

ES Modules 是 ECMAScript 官方模块化的标准，es6 实现了 ES Modules 标准。其语法是 import/export

> **浏览器端加载模块**
>
> 在浏览器端，因为其异步加载脚本文件的特性，CommonJS 无法正常加载，所以出现了 RequireJS、
> SeaJS 等为浏览器设计的模块化方案。

### ES Modules 和 CommonJS 各有各的限制

- 原生浏览器不支持 require/exports，可使用支持 CommonJS 模块规范的 Browsersify、webpack
  等打包工具，他们会将 require/exports 转换成能在浏览器中使用的代码。

- import/export 在浏览器中无法直接使用，需要在引入模块的`<script></script>`元素上添加`type="module"`
  属性。如`<script type="module" src="module.js"></script>`

- 即使 Node.js 13.2+已经支持 import/export，Node.js 官方不建议在正式环境使用，目前可以
  使用 babel 将 ES6 的模块系统编译成 CommonJS 规范（注意，语法一样，但具体还是 require/exports）

### ES Modules 与 CommonJS 对比

#### require/exports 是运行时动态加载

CommonJS 加载的是一个对象，即 module.exports 属性，该对象只有在脚本运行完才会生成。

#### import/export 是静态编译

ES Modules 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。

#### require/exports 是值的拷贝

require/exports 输出的是一个值的拷贝。也就是说，一旦输出一个值，模块内部的变化就影响
不到这个值。

#### import/export 是值引用

import/export 模块输出的是值引用。js 引擎对脚本静态分析的时候，遇到模块加载命令 import，
就会生成一个只读引用，等到脚本真正执行时，再根据这个只读引用，到模块中去取值。

若文件引用的模块值改变，require 引入的模块值不会改变，而 import 引入的模块值会改变

### require/exports 与 import/export 语法对比

#### require/exports 语法

定义模块

```
// common.js

module.exports = {
  a: 123,
  b: 'hello',
  c: function() {
    console.log('cccc')
  },
  d: {
    foo: bar
  }
}
```

使用模块

```
const com = require(./common.js)
console.log(com.a) // 123
com.c() // cccc
console.log(com.d.foo) // bar
```

### ES Modules 语法

一个 js module 可以导出一个或多个值（objects、functions、variables）使用`export`关键字

```
// uppercase.js

export default str = str.toUpperCase()
```

在这个例子中模块使用了 default export，导出了一个匿名函数，非 default export 时
必须是命名的值。
一个文件只能导出一个 default 模块

```
// module.js
const a = 1
const b = 2
const c = 3

export {
  a, b, c
}
```

`uppercase.js`模块使用 default export，导入模块时可以指定一个名称

```
import toUpperCase from './uppercase.js'

toUpperCase('test') // TEST
```

一般定义模块方式使用方式

```
import { a } from './module.js'

import { a, b } from './module.js'

import * from 'module.js'
```

也可以重命名任何导入使用`as`

```
import { a, b as two } from './module.js'

import * as mod from './module.js'
```

也可以引入 default export 和非 default export 命名导出值

```
import React, { Component } from 'react'
```

也可以这样定义模块

```
import fs from 'fs'

export default fs
export const fs
export function readFile
export { readFile, read }
exprt * from 'fs'
```

### 其他不同

- import/export 不能对引入的模块重新赋值/定义

- import/export 只能在模块顶层使用，不能在函数、判断语句等代码中使用。
  require/exports 可以

- import/export 导出的模块默认调用严格模式

```
var fun = () => {
  mistypedVaraiable = 17 // 报错，mistypeVaraible is not defined
}

export default fun
```

require/exports 默认不使用严格模式

```
module.exports = {
  fun: function() {
    mistypedVaraible = 17 // 不会报错
  }
}
```

> 严格模式是采用限制性 JavaScript 变体的一种方式

### 动态导入

import(modulePath)表达式加载模块并返回一个 promise，该 promise resolve 为一个包含其所有
导出的模块对象
我们可以在代码中的任意位置动态地使用它

```
import('/modules/my-module.js')
.then((module) => {
  // Do something with the module
})
```

> 建议不要滥用动态导入 import()，只在必要情况下采用。静态框架能更好的初始化依赖，
> 而且更有利于静态分析工具和 tree shaking 发挥作用
