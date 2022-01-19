# tsconfig

## Compiler Options

- `noImplicitAny`

In some cases where no type annotations are present, TypeScript will fall back to a
type of `any` for a variable when it cannot infer the type

if `strict` default `true`, otherwise `false`

This can cause some errors to be missed

例如

```typescript
function fn(s) {
  console.log(s.subtr(3));
}

// error
fn(42);
```

> `String.prototype.substr()`
> 不建议使用，已经被 web 标准所废弃
> `substr()` method returns a portion of the string
> starting at the specified index and extending for a given number of
> characters afterwards

## noImplicitThis

Raise error on `this` expressions with an implied `any` type

> if `strict` default `true`, otherwiese `false`

实例

```typescript
class Rectangle {
  width: number;
  height: number;

  constructor(width: number, height: number) {
    this.width = width;
    this.height = height;
  }

  getAreaFunction() {
    return function () {
      return this.width * this.height;
      // 'this' implicitly has type 'any' becase it does not have a type annotation
    };
  }
}
```

The class returns a function which to access `this.width` and `this.height`, but
the context for `this` inside the function inside `getAreaFunction` is not the
instance of the Rectangle
