# const 断言

## const 断言

```typescript
const x = { text: "hello" } as const;
```

> TypeScript 3.4 引入了一个名为 const 断言的字面值的新构造。它的语法是一个类型断言
> 用 const 代替类型名称，断言构造新的文字表达式时，我们可以向编译器发出以下信号
> 该表达式中字面量类型不应该被扩展（例如，不能从`hello`转换为字符串）
> 对象字面量获取只读属性
> 数组文字成为只读元组

## 没有类型扩展的字面类型

当我们使用关键字`const`声明一个字面量时，类型是等号右边的文字

```typescript
const x = "x"; // x has the type "x"
```

`const`关键字确保不会发生对变量进行重新分配，并且只保证该字面量的严格类型

如果我们使用`let`而不是`const`，那么该变量会被重新分配，并且类型会被扩展为字符串类型

```typescript
let x = "x"; // x has the type string
```

以下是两个不同的声明

```typescript
const x = "x"; // has the type "x"
let y = "x"; // has the type string
```

**用新的`const`功能**

```typescript
let y = "x" as const; // y has the type "x"
```
