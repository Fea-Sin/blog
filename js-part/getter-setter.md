## 属性的 getter 和 setter

有两种类型的对象属性

第一种是**数据属性**，我们已经知道如何使用它了，到目前为止，我们使用过的所有属性都是数据属性

第二中类型是**访问器属性**（accessor properties），它们本质上是用于获取和设置值的函数，但从
外部代码来看就像常规属性

### Getter 和 Setter

访问器属性由"getter"和"setter"方法表示。在对象字面量中，它们用`get`和`set`表示

```
let obj = {
  get propName() {
    // 当读取obj.propName时，getter起作用
  },

  set propName() {
    // 当执行obj.propName = value 操作时，setter起作用
  }
}
```

例如，我们有一个具有`name: "Johb"`和`surname: "Smith"`属性的对象`user`，现在想添加一个`fullName`属性，
该属性值应该为"John Smith"。当然，我们不想复制粘贴已有的信息，因此我们可以使用访问器来实现

```
let user = {
  name: "John",
  surname: "Smith",

  get fullName() {
    return `${this.name} ${this.surname}`
  }
}
console.log(user.fullName) // John Simth
```

从外表看，访问属性看起来就像一个普通属性。这就是访问器属性的设计思想，我们不以函数的方式**调用**`user.fullName`，
我们正常**读取**它（getter 在幕后运行）

截至目前，`fullName`只有一个 getter，如果尝试赋值操作`user.fullName=`将会出现错误。

我们为`user.fullName`添加一个 setter

```
let user = {
  // ...
  set fullName(value) {
    [this.name, this.surname] = value.split(" ");
  }
}

user.fullName = "Alice Cooper"

user.name // Alice
user.surname // Cooper
```

现在，我们有一个**虚拟**属性，它是可读写的

### 访问器描述符

访问器的描述符与数据属性的不同

对于访问器属性，没有`value`和`writable`，但是有`get`和`set`函数

访问器描述符有

- get: 一个没有参数的函数，在读取属性时工作
- set: 带有一个参数的函数，当属性被设置时调用
- enumerable: 与数据属性的相同
- configurable: 与数据属性的相同

使用`defineProperty`创建一个`fullName`访问器，我们可以使用`get`和`set`来传递描述符

```
let user = {
  name: "John",
  surname: "Simth"
}

Object.defineProperty(user, 'fullName', {
  get() {
    return `${this.name} ${this.surname}`
  },
  set(value) {
    [this.name, this.surname] = value.split(" ")
  }
})

user.fullName // John Smith
for(let key in obj) alert(key) // name, surname
```

注意，一个属性要么是访问器（具有 get/set 方法），要么是数据属性（具有 value），但不能两者都是

### 更聪明的 getter/setter

getter/setter 可以作为**真实**属性的包装器，以便对它进行更多的控制

例如，我们想禁止太短的 user 的 name，我们可以创建一个 setter name，并将值存储在一个单独属性`_name`中

```
let user = {
  get name() {
    return this._name
  }
  set name(value) {
    if (value.length < 4) {
      console.log("Name is too short, need at least 4 characters")
      return;
    }
    this._name = value
  }
}
```

从技术上将，外部代码可以使用`user._name`直接访问 name，但是，这是一个众所周知的约定，`_`开头的属性是
内部属性，不应该从对象外部进行访问

### 兼容性

访问器的一大用途是，它们允许随时通过使用 getter 和 setter 替换**正常的属性**，来控制和调整这些属性的行为
