# Node.js Package.json Exports Field

## `exports` field

`exports` field 提供了一种暴露你 package modules 的方式，package modules 可以有不同的
环境，不同的引入方式，或者权限的限制

- 包的不同环境，如 NodeJS、browser，甚至`node-env-like`像`production/development`，例如有些场景，
  如 React / Vue 项目在开发时需要有 warning 提示，而在生产环境时却不需要

- 不同的引入方式，EcmaScript Module 方式 `import ... from "..."`，CommonJS`const ... = require(...)`

## 如何使用

package 库的项目结构

```
my-some-lib
  |-- lib/
  |    |-- whole-lib.browser.js (iife format)
  |    |-- public-module-a.cjs  (commonjs format)
  |    |-- public-module-a.mjs  (esmodule format)
  |    |-- public-module-b.cjs
  |    |-- public-module-b.mjs
  |    |-- internals/
  |         |-- private-module-c.cjs
  |         |-- private-module-c.mjs
  |-- package.json
```

我们暴露了`module-a`和`module-b`限制了`module-c`包的访问，并且提供了 CommonJS 和 ESModule
的引入方式

exports field：

```json
{
  "name": "my-some-lib",
  "exports": {
    ".": {
      "browser": {
        "default": "./lib/whole-lib.browser.js"
      }
    },
    "module-a": {
      "import": "./lib/public-module-a.mjs",
      "require": "./lib/public-module-a.cjs"
    },
    "module-b": {
      "import": "./lib/public-module-b.mjs",
      "require": "./lib/public-module-b.cjs"
    }
  }
}
```

> `exports`每个路径都是相对于 package root，每个路径必须以`./`开始

## 包如何引入

```js
// CommonJS
const moduleA = require("my-some-lib/module-a");

// ESModule
import moduleA from "my-some-lib/module-a";

// 如下方式会出现问题
const moduleA = require("my-some-lib/lib/public-module-a");
const moduleC = require("my-some-lib/lib/internals/private-module-c");
import moduleA from "my-some-lib/lib/public-module-a";
```
