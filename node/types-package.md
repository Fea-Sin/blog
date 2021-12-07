# 发布携带 TypeScript definitions 的 npm package

发布 TypeScript 模块到 NPM，声明文件有两种方式来发布声明文件

1. 与模块包捆绑在一起发布
2. 声明文件发布到 npm 上的[@types organization](https://www.npmjs.com/~types)

我们推荐第一种方式，这样的话，你的声明文件与 JavaScript 总是在一起传递。
另外，TypeScript 使用者可以引入我们的包和 type definitions

## 发布携带 type definitions 的 npm 包

如果你的包有一个 main js 文件，你还需要在 package.json 里指定主声明文件。设置`types`属性指向声明文件

```json
{
  "name": "awesome",
  "version": "1.0.0",
  "main": "./lib/main.js",
  "types": "./lib/main.d.ts"
}
```

> 注意，`types`与`typings`具有相同的意义，都可以使用

> 同样注意的是，如果主声明文件名是`index.d.ts`并且位置在包的根目录（与 `index.js`并列），可以不需要使用
> `types`属性指定

## 依赖

需要依赖的声明包，要在`package.json`的`dependencies`里指明，例如，我们写了一个包它依赖 Browserify 和 TypeScript

```json
{
  "name": "browserify-typescript-extension",
  "version": "1.0.0",
  "main": "index.js",
  "types": "index.d.ts",
  "dependencies": {
    "browserify": "latest",
    "@types/browserify": "latest",
    "typescript": "next"
  }
}
```

这里，我们的包依赖 browserify 和 typescript 包。browserify 没有把它的声明文件捆绑在它的 npm 包里。
所以我们需要依赖于`@types/browserify`得到它的声明文件。typescript 则把声明文件放在了 npm 包里，因此
我们不需要依赖额外的包。

我们的包要从这两个包里暴露声明文件，因此`browserify-typescript-extension`的用户也需要依赖，正因此，
我们使用`dependencies`而不是`devDependencies`

如果你的类型声明依赖于另一个包

- 不要把依赖的包放进你的包里，保持它们在各自的文件里
- 不要将声明拷贝到你的包里

## 如何让 TypeScript 自动生成 type definitions

TypeScript 编译器可以从你的 TypeScript Code 发布（emit）definitions，它会将`.ts`文件中的`definitions`
生成`.d.ts`文件，但是`.d.ts`文件不会触发 emit，仅项目中的`.ts`文件可以 emit

要实现上述功能需要两步

1. use CLI argument `--declaration`

```bash
tsc --declaration
```

2. 在`tsconfig.json`中指定`declaration`

```json
{
  "compilerOptions": {
    "declaration": true
  }
}
```

## 明确的导出需要的 types 在你的 package 中

项目目录

```
project-root
  |-- src
  |    |-- index.ts
  |    |-- types.ts
```

```typescript
// types.ts
interface OPtions {
  message: string;
}
```

```typescript
// index.ts
export function log(opts: Options): void {
  console.log(opts.message);
}
```

执行 TypeScript 编译

```
project-root
  |-- dist
  |    |-- index.js
  |    |-- index.d.ts
  |    |-- types.js
  |    |-- types.d.ts
```

此时`index.d.ts`文件如下

```typescript
export declare function log(opts: Options): void;
```

此时，你并没有引入`Options`interface， 如果有人使用你的包，此时`opts`as type `any`

**如何才能解决上述问题**

```typescript
// types.ts
export interface Options {
  message: string;
}
```

```typescript
// index.ts
import { Options } from "./types";
export function log(opts: Options): void {
  console.log(opts.message);
}
```

你可以新建一个`index.d.ts`作为你的声明文件

```typescript
import { Options } from "./types";

export declare function log(opts: Options): void;

export * from "./types";
```
