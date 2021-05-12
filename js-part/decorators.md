## JS 装饰器

在 ES6 中增加了类对象的相关定义和操作（比如 class 和 extends），我们在多个不同类之间共享或者
扩展一些方法或者行为的时候，并不是那么优雅。这个时候，我们就需要一种更优雅的方法来帮助我们完成
这些事情

### 装饰器语法

装饰器 decorator 接受一个参数，也就是被装饰的目标方法，处理完扩展的内容以后再返回一个方法，供以后调用，
同时也失去了对原方法对象的访问。当我们对某个方法应用了装饰以后，其实就改变了被装饰方法的入口引用，使其
重新指向了装饰器返回的方法的入口点，从而来实现我们对原方法的扩展、修改等操作

### ES7 的装饰器

ES7 中的 decorator 同样借鉴了这个语法糖，不过依赖于 ES5 的`Object.defineProperty`方法

---

### Object.defineProperty

`Object.defineProperty()`方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回这个对象。

该方法允许精确添加或修改对象的属性。通过赋值来添加的普通属性会创建默认可枚举属性（for...in 或 Object.keys()），
通过`Object.defineProperty`可以更好的控制额外的细节。默认情况下，使用`Object.defineProperty()`添加的属性值
是不可变的

语法

```
Object.defineProperty(obj, prop, descriptor)
```

- obj 要在其上定义属性的对象
- prop 要定义或修改的属性的名称
- descriptor 属性描述符
- 返回值 被修改后的对象

### 属性描述符

对象里目前存在的属性描述符有两种主要形式，**数据描述符**和**存取描述符**

- 数据描述符，是一个具有值的属性，该值可能是可写的，也可能是不可写的
- 存取描述符，是由 getter-setter 函数对描述的属性

描述符必须是两种形式之一，不能同时是两者

#### 数据描述符和存取描述符均具有以下可选键值

**configurable**

当且仅当该属性的 `configurable` 为 true 时，该属性描述符才能够被改变，同时该属性也能从对应的对象上被删除。
`configurable`默认为 false

**enumerable**

`enumerable`定义了对象的属性是否可以在`for...in`循环和`Object.keys()`中被枚举
当且仅当该属性的 enumerable 为 true 时，该属性才能出现在对象的枚举属性中。默认为 false

#### 数据描述符同时具有以下可选键值

**value**

该属性对应的值。可以是任何有效的 JavaScript 值（数值、对象、函数等）。默认为`undefined`

**writable**

当且仅当该属性的`writable`为 true 时，value 才能被赋值运算符改变。默认为 false

#### 存取描述符同时具有以下可选键值

**get**

一个给属性提供`getter`的方法，如果没有`getter`则为`undefined`。getter 方法返回值被用作属性值。默认是 undefined

**set**

一个给属性提供`setter`的方法，如果没有 setter 则为 undefined。setter 方法接受唯一参数，该方法的返回值为该属性的新值。
默认为 undefined

> 如果一个描述符不具有 value、writable、get、set 任意一个关键字
>
> 那么它将被认为是一个数据描述符

---

### 类的装饰

```
@testable
class MyTestableClass {
  // ...
}

function testable(target) {
  target.isTestable = true
}

MyTestableClass.isTestable // true
```

上面代码中，`@testable`就是一个装饰器。它修改了 MyTestableClass 这个类的行为，为它加上了
静态属性`isTestable`。testable 函数的参数 target 是 MyTestableClass 类本身

基本上，类装饰器的行为就是下面这样

```
@decorator
class A {}

// 等同于
A = decorator(A) || A
```

**装饰器是一个对类进行处理的函数，装饰器函数的第一个参数，就是所要装饰的目标类**

如果需要额外参数，可以使用高阶函数的方式传入

```
function testable(isTestable) {
  return function(target) {
    target.isTestable = isTestable
  }
}

@testable(false)
class MyTestableClass {}

MyTestableClass.isTestable // false
```

装饰器对类的行为的改变，是代码编译时发生的，而不是在运行时。也就是说，
**装饰器本质是编译时执行的函数**

前面的例子是为类添加一个静态属性，如果想添加实例属性，可以通过目标类的 prototype 对象操作

```
// mixins.js
export function mixins(...list) {
  return function(target) {
    Object.assign(target.prototype, ...list)
  }
}

// main.js
import { mixins } from './mixins'

const Foo = {
  foo() {
    console.log('foo')
  }
}
@mixins(Foo)
class MyClass {}

let obj = new MyClass()

obj.foo() // 'foo'
```

方法的装饰

装饰器不仅可以装饰类，还可以装饰类的**属性**

```
class Person {
  @readonly
  name() {
    return `${this.first} ${this.last}`
  }
}
```

属性装饰器一共可以接受三个参数

```
function readonly(target, name, descriptor) {
  // descriptor 对象原来的值如下
  // {
  //    value: specifiedFunction
  //    enumerable: false
  //    configurable: true
  //    writable: true
  // }
  descriptor.writable = false
  return descriptor
}

readonly(Person.prototype, 'name', descriptor)
// 类似于
Object.defineProperty(Person.protoype, 'name', descriptor)
```

- 属性装饰第一个参数是**类的原型对象**，这不同于类的装饰，类装饰第一个参数是类本身

- 第二个参数是，所装饰的属性名

- 第三个参数是，该属性的描述符

### 函数的装饰

装饰器只能用于类和类的属性，不能用于函数，因为函数存在提升

如果一定要装饰函数，可以采用高阶函数的形式直接执行

```js
function doSomething(name) {
  console.log("Hello, " + name);
}

function loggingDecorator(wrapped) {
  return function () {
    console.log("Starting");
    const result = wrapped.apply(this, arguments);
    console.log("Finished");
    return result;
  };
}
const wrapped = loggingDecorator(doSomething);
```

### 使用场景

点击菜单时，进行事件拦截，若该菜单有新功能更新，则弹框提示

```js
/**
 * @description 在点击时，如果有新功能提醒，则弹框提示
 * @param code 新功能的code
 * @returns
 */
const checkRecommanFunc = (code) => (target, property, descriptor) => {
  let desF = descriptor.value;
  descriptor.value = function (...args) {
    let recommandFuncModalData = SYSTEM.recommandFuncCodeMap[code];

    if (recommandFuncModalData && recommandFuncModalData.id) {
      setTimeout(() => {
        this.props.dispatch(
          {
            type: "global/setRecommanFuncModalData",
          },
          recommandFuncModalData
        );
      }, 1000);
    }
    desF.apply(this, args);
  };
  return descriptor;
};
```

### 加 loading

我们可能需要向后台请求数据时，页面出现 loading，这个时候，可以使用装饰器优雅实现

```
@autobind
@loadingWrap()
async handleSelect(params) {
  await this.props.dispatch({
    type: 'product_list/setQueryParams',
    queryParams: params
  })
}
```

```jsx
// loadingWrap 函数
export default loadingWrap() {
  const defaultLoading = (
    <div>加载中...</div>
  )

    return function(target, property, descriptor) {
    const raw = descriptor.value
    descriptor.value = function(...args) {
      Toast.info(defaultLoading)
      const res = raw.apply(this, args)
      res.finally(() => {
        Toast.hide()
      })
    }

    return descriptor
  }
}
```
