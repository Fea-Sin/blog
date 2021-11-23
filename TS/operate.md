# TypeScript 中的`??`, `?:`, `?.`, `!.`

## `?:`

可选参数，可以理解为参数自动加上`undefined`

```typescript
function echo(x: number, y?: number) {
  return x + (y || 0);
}

echo(1); // 1
echo(1, null); // error, null is not assignable to number | undefined
```

## `??`

`??`和`||`的意思有点像，但是又有区别`??`比较严谨，当值是`0`时仍然有效

```js
null || 5; // 5
null ?? 5; // 5

undefined || 5; // 5
undefined ?? 5; // 5

0 || 5; // 5
0 ?? 5; // 0
```

## `?.`

`?.`的意思基本和`&&`一样

```js
const a = {
  b: { c: 7 },
};

a?.b?.c; // 7

a && a.b && a.b.c; // 7
```

## `!.`

`!.`是断言，告诉 ts 这个对象一定有值

```typescript
const inputRef = useRef<HTMLInputElement>(null);

const value: string = inputRef.current!.value;
```
