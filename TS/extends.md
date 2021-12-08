# extends

TypeScript 的属性继承有两种方式

- `extends` 只能用于`interface`和`class`

- `Intersection Types` 类型交集

## extends

在`interface` 上使用 `extends` 关键字，可以有效的复制类型成员，并且可以增加任何我们需要的成员

### 单接口继承

```typescript
interface BasicAddress {
  name?: string;
  street: string;
  city: string;
  country: string;
  postalCode: string;
}

interface AddressWithUnit extends BasicAddress {
  unit: string;
}
```

### 多接口继承

```typescript
interface Colorful {
  color: string;
}

interface Circle {
  radius: number;
}

interface ColorfulCircle extends Colorful, Circle {}

const mycc: ColorfulCircle = {
  color: "red",
  redius: 42,
};
```

## Intersection Types (交集类型)

`interface`允许我们通过`extends`其他的类型创建一个新的类型。
TypeScript 提供了另一种结构，结合当前存在的`object types`，这就是 `Intersection Types`

```typescript
interface Colorful {
  color: string;
}
interface Circle {
  radius: number;
}

type ColorfulCircle = Colorful & Circle;
```

## class implements interface

示例

```typescript
interface Mailable {
  send(email: string): boolean;
  queue(email: string): boolean;
}

interface FutureMailable extends Mailable {
  later(email: string, after: number): boolean;
}

class Mail implements FutureMailable {
  later(email: string, after: number): boolean {
    console.log(`Send email to ${email} in ${after} ms`);
    return true;
  }

  send(email: string): boolean {
    console.log(`Send email to ${email}`);
    return true;
  }

  queue(email: string): boolean {
    console.log(`Queue an email to ${email}`);
    return true;
  }
}
```

## Interfaces extending classes

TypeScript 允许接口`extends` a `class`，接口继承 `class` 的属性和方法，接口不光继承
`public`成员，`private`和`protected`成员同样可以继承

如果一个接口继承了一个`class`的`private`和`protected`成员，那么只有当前类和当前类的子类可以
`implements`这个接口
