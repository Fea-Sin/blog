## Vue Loader

Vue Loader 是一个 webpack 的 loader，它允许你以一种名为单文件组件(SFC)的格式
撰写 Vue 组件

```
<template>
  <div class="example">{{ msg }}</div>
</template>
<script>
export default {
  data() {
    return {
      msg: 'Hello world'
    }
  }
}
</script>
<style>
.exmple {
  color: red;
}
</style>
```

Vue Loader 还提供了很多酷炫的特性

- 允许为 Vue 组件的每个部分使用其它的 webpack loader，例如在`<style>`的部分
  使用 sass 和在`<template>`的部分使用 Pug

- 允许在一个`.vue`文件中使用自定义快，并对其运用自定义的 loader 链

- 使用 webpack loader 将`<style>`和`<template>`中引用的资源当作模块依赖来处理

- 为每个组件模拟出 scoped css

- 在开发过程中使用热重载来保持状态

webpack 和 Vue Loader 的结合为你提供了一个现代、灵活且极其强大的前端
工作流
