# TypeScript is 关键字

`is`关键字，被称为类型谓词，用来判断一个变量属于某个接口或类型。如果需要封装一个类型判断函数，
你应该想到它

`is`关键字一般用于函数返回值类型中，判断参数是否属于某一类型，并根据结果返回对应的布尔值

## 举例说明

```typescript
function isString(s: unknown): boolean {
  return typeof s === "string";
}

function toUpperCase(x: unknown) {
  if (isString(x)) {
    x.toUpperCase(); // Error, Object is of type "unknown"
  }
}
```

由于函数嵌套 TypeScript 不能进行正确的类型判断，TypeScript 抛出了一个错误提示，一个 unknown 类型的对象
不能进行 toUpperCase() 操作。按理说我们已经在上一行确定了参数`x`为`string`类型

## 使用

```typescript
const isString = (s: unknown): s is string => typeof val === "string";

function toUpperCase(x: unknown) {
  if (isString(x)) {
    x.toUpperCase();
  }
}
```
