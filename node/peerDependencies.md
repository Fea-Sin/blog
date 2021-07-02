## 一文读懂 Peer Dependencies

npm 是一个优秀的包管理器，特别是它对依赖的管理。举个例子，我的项目依赖`request`(version 2)
和`some-other-library`，但是`some-other-library`也依赖`request` (version 1)。此时的
依赖树是这样的

```
 |----request@2.12.0
 |----some-other-library@1.2.3
      |------request@1.9.9

```

此时`some-other-library`有自己的 copy of `request`(version 1)，而不是引用
我们项目中的 package v2，Every thing 工作的都很好

### 问题在插件

在有的使用场景下会出现问题，就是在开发一些流行库插件的时候

比较常见的 Node.js package ecosystem

- Grunt plugins

- Chai plugins

- Express middleware

本质上，插件一般都服务于一些流行库，通常情况下插件不会依赖它们的流行库。例如，
grunt plugins 不会`require("grunt")`，即使插件显式依赖它们的流行库`dependency`，
项目运行的时候也不会使用。这样就有可能出现项目使用`Grunt`与插件所使用版本冲突的情况

### 解决方案：Peer Dependencies

我们需要表现一种依赖关系，between 插件和它们的流行库。类似于"本插件仅支持 version 1.2.x，
需要跟特定的流行库一块使用"，这是一种 `peer dependency`

### 使用 Peer Dependencies

Peer Dependencies 使用非常简单，需要确定 peer-denpend 流行库的版本

```json
{
  "name": "chai-as-promised",
  "peerDependencies": {
    "chai": "1.x"
  }
}
```

需要注意的是，不像这些普通的 `dependencies`，需要宽泛一些，一般不要锁定单一版本，
如果一个 Chai 插件依赖于 1.4.1，而另一个依赖 1.5.0，那就不能同时共用

最好的方式是遵循 `semver`，如果你的插件工作于`1.x`，使用`"1.x"`

如果你的插件依赖 `1.5.2`，可以使用`">= 1.52 < 2"`

```json
"peerDependencies": {
  "chai": "1.x"
}
```

or

```json
"peerDependencies": {
  "chai": ">= 1.5.2 <2"
}
```
