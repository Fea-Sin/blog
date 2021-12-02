# TypeScript unknown 类型

TypeScript 3.0 引入了新的`unknown`类型，它是`any`类型对应的安全类型

`unknown` 和 `any`的主要区别是`unknown`类型会更加严格，在对`unknown`类型的值执行大多数操作之前，我们必须进行
某种形式的检查。而在对`any`类型的值执行操作之前，我们不必进行任何检查

这篇文章主要关注于`unknown`类型的实际应用，以及包含了与`any`类型的比较

## `any`类型

让我们首先看`any`类型，这样我们就可以更好地理解引入`unknown`类型背后的动机

自从 TypeScript 在 2012 年发布第一个版本以来`any`类型就一直存在，它代表所有可能的
JavaScript 值，基本类型，对象，数组，函数，Error，Symbol，以及任何你可能定义的值

在 TypeScript 中，任何类型都可以被归为 any 类型。这让`any`类型成为了类型的`顶级类型`

示例

```typescript
let value: any;

value = true; // ok
value = 42; // ok
value = "hello world"; // ok
value = []; // ok
value = {}; // ok
value = Math.random; // ok
value = null; // ok
value = undefined; // ok
value = new TypeError(); // ok
value = Symbol("type"); // ok
```

`any`类型本质上是类型系统的一个逃逸舱。作为开发者，这给了我们很大的自由，TypeScript 允许我们对`any`
类型的值执行任何操作，而无需事先执行任何形式的检查

在上述例子中，变量`value`被定义成类型`any`，TypeScript 认为以下所有操作都是类型正确的

```typescript
let value: any;

value.foo.bar; //ok
value.trim(); // ok
value(); //ok
new value(); // ok
value[0][1]; // ok
```

使用`any`类型，可以很容易地编写类型正确但是执行异常的代码，如果我们使用`any`类型，就无法
享受 TypeScript 大量的保护机制

## `unknown`类型

就像所有类型都可以被归为`any`，所有类型也都可以被归为`unknown`，这使得`unknown`成为 TypeScript 类型
系统的另一种顶级类型

示例

```typescript
let value: unknown;

value = true; // ok
value = 42; // ok
value = "hello world"; // ok
value = []; // ok
value = {}; // ok
value = Math.randon; // ok
value = null; // ok
value = undefined; // ok
value = new TypeError(); // ok
value = Symbol("type"); // ok
```

对`value`变量的所有赋值都被认为是类型正确的

当我们将类型为`unknown`的值赋值给其他类型的变量时会发生什么？

```typescript
let value: unknown;

let value: unknown = value; // ok
let value: any = value; // ok
let value: boolean = value; // Error
let value: number = value; // Error
let value: string = value; // Error
let value: object = value; // Error
let value: any[] = value; // Error
let value: Function = value; // Error
```

`unknown`类型只能被赋值给`any`和`unknown`类型。直观的说，这是有道理的，只有能够保存任意类型值的容器才能
保存`unknown`类型的值

我们看看对`unknown`类型执行操作会发生什么？

```typescript
let value: unknown;

value.foo.bar; // Error
value.trim(); // Error
value(); // Error
new value(); // Error
value[0][1]; // Error
```

这是`unknown`类型的主要价值主张，TypeScript 不允许我们对类型`unknown`的值执行任意操作。我们必须
首先执行某种类型检查以缩小我们正在使用的值的类型范围

## 缩小`unknown`类型范围

我们可以通过不同的方式将`unknown`类型缩小为更具体的类型范围，包括`typeof`运算符，`instanceof`
运算符和自定义类型保护函数。

以下示例说明了`value`如何在两个`if`语句分支中获得更具体的类型

```typescript
function stringifForLogging(value: unknown): string {
  if (typeof value === "function") {
    const functionName = value.name || "anonymous";
    return `[function ${functionName}]`;
  }

  if (value instanceof Date) {
    return value.toISOString();
  }
}
```

除了使用`typeof`或`instanceof`运算之外，我们还可以使用`自定义类型保护函数`缩小
`unknown`类型范围

```typescript
function isNumberArray(value: unknown): value is number[] {
  return (
    Array.isArray(value) &&
    value.every((element) => typeof element === "number")
  );
}

if (isNumberArray(value)) {
  // `value` has type `number[]`
  const max = Math.max(...value);
}
```

## 对`unknown`类型使用类型断言

上面我们已经看到如何使用`typeof`, `instanceof`和自定义类型保护函数来说服 TypeScript
编译器某个值具有某种类型。这是`unknown`类型指定为更具体类型的安全推荐的方法

如果要强制编译器信任类型为`unknown`的值为给定类型，可以使用`类型断言`

```typescript
const value: unknown = "hello world";
const someString: string = value as string;
```

类型检查器假定你更了解并相信你在类型断言中使用的任何类型都是正确的，如果你犯了错误
指定了错误的类型，运行时可能会抛出错误

```typescript
const value: unknown = 42;
const someString: string = value as string;
const other = someString.toUpperCase(); // Error
```
