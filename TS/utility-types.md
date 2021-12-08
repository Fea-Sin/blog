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
