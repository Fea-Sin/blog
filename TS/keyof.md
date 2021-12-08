# `keyof` type operator

`keyof`操作符，takes an object type，产生 object keys 的 string or numberic 字面量

```typescript
type Point = { x: number; y: number };

type P = keyof Point;
// type P = "x" | "y"
```

`index signature` the type is `string` or `number`，`keyof`将会返回这些 type

```typescript
type Arrayish = { [n: number]: unknown };

type A = keyof Arrayish; // type A = number
```

```typescript
type Mapish = { [k: string]: boolean };

type M = keyof Maoish;
// type M = string | number

// M is `string | number`，这是因为JavaScript object keys 总是隐式改为`string`
// 所以，`obj[0]` 和 `obj["0"]`总是相同的
```
