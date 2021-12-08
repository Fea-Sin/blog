# `typeof` type operator

JavaScript 早就有`typeof`操作符，通常用于表达式中

```js
typeof "hello world"; // "string"
```

TypeScript 增加了一个`typeof`操作符，通常用于`type context`，用于获取变量或属性的 type

```js
let s = "hello";

let n: typeof s;
// type n = string
```

## 与`ReturnType`联合使用

```typescript
function f() {
  return { x: 10, y: 3 };
}

type P = ReturnType<typeof f>;
// P = {
//   x: number;
//   y: number;
// }
```
