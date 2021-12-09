# Utility Types

TypeScript 提供了几个工具类以方便类型的转换，这些工具是全局可用的

## `Partial<Type>`

版本: `2.1`

所有的属性变为可选

示例

```typescript
interface Todo {
  title: string;
  description: string;
}

function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
  return { ...todo, ...fieldsToUpdate };
}

const todo1 = {
  title: "organize desk",
  description: "clear clutter",
};

const todo2 = updateTodo(todo1, {
  description: "throw out trash",
});
```

## `Required<Type>`

版本: `2.8`

所有属性都是必须的。与`Partial`相反

示例

```typescript
interface Props {
  a?: number;
  b?: string;
}

const obj: Props = { a: 5 };

const obj2: Required<Props> = { a: 5 }; // Error

// Property 'b' is missing
```

## `Record<Keys, Type>`

版本: `2.1`

属性 key 是`Keys`类型，属性 value 是`Type`类型

示例

```typescript
interface CatInfo {
  age: number;
  breed: string;
}

type CatName = "miffy" | "boris" | "mordred";

const cats: Record<CatName, CatInfo> = {
  miffy: { age: 10, breed: "Persian" },
  boris: { age: 5, breed: "Maine Coon" },
  mordred: { age: 16, breed: "British Shorthair" },
};
```

## `ReturnType<Type>`

版本: `2.8`

获取`function type`的返回 Type

示例

```typescript
declare function f1(): { a: number; b: string };

type T0 = ReturnType<() => string>; // type T0 = string

type T1 = ReturnType<(s: string) => void>; // type T1 = void

type T2 = ReturnType<<T>() => T>; // type T2 = unknown

type T3 = ReturnType<<T extends U, U extends number[]>() => T>; // type T3 = number[]

type T4 = ReturnType<typeof f1>;
/*
 * 
  type T4 = {
    a: number;
    b: string;
  }
 */

type T5 = ReturnType<any>; // type T5 = any

type T6 = ReturnType<never>; // type T6 = never

type T7 = ReturnType<string>; // Error; type T7 = any
```

## `Extract<Type, Union>`

版本: `2.8`

相对于 `Union` 从 `Type` 提取交集 types

```typescript
type T0 = Extract<"a" | "b" | "c", "a" | "f">;

// type T0 = "a"
```

```typescript
type T1 = Extract<string | number | (() => void), Function>;

// type T1 = () => void
```
