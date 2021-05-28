## process.env.NODE_ENV

技术上讲`NODE_ENV`是一个由 Node.js 暴露给执行脚本的系统环境变量。通常用于决定在开发环境
与生产环境下，server tools（服务端工具）、build script（构建脚本）和 client side libraries（客户端库）
的行为。

如果你正在使用像`react`这样的 library，那么在添加 DefinePlugin 插件后，你应该看到 bundle 大小显著下降。
要注意，任何位于`/src`的本地代码都可以关联到`process.env.NODE_ENV`环境变量

```js
import { cube } from "./math.js";

if (process.env.NODE_ENV !== "production") {
  console.log("Looks like we are in development mode!");
}

function component() {
  const element = document.createElement("pre");
  element.innerHTML = ["Hello webpack!", "5 cubed is equal to " + cube(5)].join(
    "\n\n"
  );

  return element;
}
document.body.appendChild(component());
```
