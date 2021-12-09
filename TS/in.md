# TypeScript `in`

`in` 是一个标准的 JavaScript 操作符

如果指定的属性在对应的 object 中，则返回`true`，语法如下

```js
propName in barObject;
```

> propName 是一个字符串或数字表达式，对应对象的属性名称或`array index`

## 在 TypeScript 中

在 TypeScript 中`in`依旧是扮演`type guard(类型守卫)`的角色

```typescript
interface A {
  x: number;
}
interface B {
  y: string;
}

let q: A | B = `...`;

if ("x" in q) {
  // q: A
} else {
  // q: B
}
```
