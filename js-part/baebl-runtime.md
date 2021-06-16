## @babel/plugin-transform-runtime 到底是什么

很多初学者刚接触 babel 的时候，通常会看到这样一个报错

```
Uncaught ReferenceError: regeneratorRuntime is not defined
```

这个报错表面上是由于`async function`语法被 babel 转译之后的代码使用了`regeneratorRuntime`
这个变量，但是这个变量在最终的代码里未定义造成的报错

babel 在转译的时候，会将源代码分成`syntax`和`api`两部分来处理

- syntax 类似于展开对象、optional chain、let、const 等语法

- api 类似于`[1, 2, 3].includes()`等函数、方法

**实例**

```js
const array = [1, 2, 3, 4];

array.includes((item) => item > 2);
```

转译后

```js
"use strict";

var array = [1, 2, 3, 4];
array.includes(function (item) {
  return item > 2;
});
```

babel 很轻松就把`syntax`转译好了，但`api`并没有做任何处理，babel 转译后的
代码如果在不支持`includes`这个方法的浏览器里运行就会报错

babel 使用`polyfill`来处理 api，`@babel/preset-env`中有一个配置选项`useBuiltIns`，
用来告诉 babel 如何处理 api，由于这个选项默认值是 false，即不处理 api，所以上面代码
转译后没有处理`includes`这个方法

设置 `useBuiltIns`的值为`entry`，同时在源代码的最上方手动引入`@babel/polyfill`这个库，该
库一共分两部分，第一部分是`core-js`，第二部分是`regenerator-runtime`

```JSON
{
  "presets": [
    ["@babel/preset-env", {
      "useBuiltIns": "entry",
      "debug": true
    }]
  ]
}
```

安装`@babel/polyfill`

```
npm install --save @babel/polyfill
```

源码

```js
import "@babel/polyfill";

const array = [1, 2, 3, 4];

array.includes((item) => item > 2);
```

编译后

```js
"use strict";

require("core-js/modules/es6.array.copy-within.js");

require("core-js/modules/es6.array.every.js");
// ....
// ....
// ....
require("regenerator-runtime/runtime.js");

var array = [1, 2, 3, 4];
array.includes(function (item) {
  return item > 2;
});
```

这种模式下，babel 会将所有的 polyfill 全部引入这样会导致结果包非常大，而我们这里仅需要
`includes`一个方法而已。

正确的做法是使用**按需加载**，将 useBuiltIns 改为`usage`，babel 就可按需加载 polyfill，
并且不需要**手动引入@babel/polyfill**

```JSON
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "useBuiltIns": "usage",
        "debug": true
      }
    ]
  ]
}
```

编译后

```js
"use strict";

require("core-js/modules/es7.array.includes.js");

var array = [1, 2, 3, 4];
array.includes(function (item) {
  return item > 2;
});
```

到目前为止，上面 babel 配置还存在两个问题

从上面的转译结果可以看到，`includes`这 api 直接`require`了

```js
require("core-js/modules/es7.array.includes.js");
```

**问题一**
babel 的 polyfill 机制是，对于`Array.from`等静态方法，直接在`global.Array`上添加，
对于`includes`等实例方法，直接在`global.Array.prototype`上添加

这样直接修改了全局变量的原型，有可能带来意想不到的问题

这个问题在开发第三方库的时候尤其重要，因为我们开发的第三方库修改了全局变量，有可能跟另一个
三方库发生冲突

**问题二**
babel 转译 `syntax`时，有时候会使用一些辅助函数来帮忙转

```js
class Person {
  constructor(name) {
    this.name = name;
  }

  toSay() {
    console.log(`The person name is ${this.name}`);
  }
}
```

转译后

```js
"use strict";

require("core-js/modules/es6.function.name.js");

require("core-js/modules/es6.object.define-property.js");

function _classCallCheck(instance, Constructor) {
  if (!(instance instanceof Constructor)) {
    throw new TypeError("Cannot call a class as a function");
  }
}

function _defineProperties(target, props) {
  for (var i = 0; i < props.length; i++) {
    var descriptor = props[i];
    descriptor.enumerable = descriptor.enumerable || false;
    descriptor.configurable = true;
    if ("value" in descriptor) descriptor.writable = true;
    Object.defineProperty(target, descriptor.key, descriptor);
  }
}

function _createClass(Constructor, protoProps, staticProps) {
  if (protoProps) _defineProperties(Constructor.prototype, protoProps);
  if (staticProps) _defineProperties(Constructor, staticProps);
  return Constructor;
}

var Person = /*#__PURE__*/ (function () {
  function Person(name) {
    _classCallCheck(this, Person);

    this.name = name;
  }

  _createClass(Person, [
    {
      key: "toSay",
      value: function toSay() {
        console.log("The person name is ".concat(this.name));
      },
    },
  ]);

  return Person;
})();
```

class 语法中，babel 自定义了`_classCallCheck`这个辅助函数，如果项目中有 100 个文件，
每个文件都有一个 class，那么最终打包的产物就会有 100 个`_classCallCheck`函数，他们长相
和功能一摸一样

### @babel/plugin-transform-runtime 这插件就是解决上面提到的两个问题的

安装

```
npm install --save-dev @babel/plugin-transform-runtime

npm install --save @babel/runtime
```

配置

```JSON
{
    "presets": [
    [
      "@babel/preset-env",
      {
        "useBuiltIns": "usage",
        "debug": true
      }
    ]
  ],
  "plugins": ["@babel/plugin-transform-runtime"]

}
```

@babel/plugin-transform-runtime 的作用是将 helper 和 polyfill 改为从一个统一的地方引入，
并且引入的对象和全局变量是完全隔离的，这样就解决了上面的两个问题

```js
export function* helloSaga() {
  console.log("Hello, Saga");
}
```

最终转译

```js
"use strict";

require("core-js/modules/es6.object.define-property.js");

var _interopRequireDefault = require("@babel/runtime/helpers/interopRequireDefault");

Object.defineProperty(exports, "__esModule", {
  value: true,
});
exports.helloSaga = helloSaga;

var _regenerator = _interopRequireDefault(
  require("@babel/runtime/regenerator")
);

var _marked = /*#__PURE__*/ _regenerator["default"].mark(helloSaga);

function helloSaga() {
  return _regenerator["default"].wrap(function helloSaga$(_context) {
    while (1) {
      switch ((_context.prev = _context.next)) {
        case 0:
          console.log("Hello, Saga");

        case 1:
        case "end":
          return _context.stop();
      }
    }
  }, _marked);
}
```
