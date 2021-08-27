# Vue 递归组件

## 简介

有人说递归很难容融会贯通。一个简单的示例是`recursive function`(递归函数)，递归函数是自我调用的函数，
意思是函数在执行某个时刻时调用自己。

`recursion` 两条规则

- A base case（基本规则）, 在某个条件下停止递归

- A set of rules, 将其它情况归结到 base case

不能确定哪一条规则更重要，如果没有基本规则，递归将无限循环，如果不能减少其它情况到基本规则，我们
就不能达到我们的目的

## 递归和 Vue 组件

递归在 Vue 中非常有用，甚至我们可以基于上述规则将递归应用于任何框架。所以基于上述的定义，我们可以
说递归组件是一种调用自己的组件。

何时使用，当遇到使用相同 `template structure`，并且是分层的数据结构，此时可以使用递归。
例如，以树形结构展示嵌套文件夹结构，网站的评论，嵌套的菜单，任何父元素和子元素有相同结构的组件。

## 一个实际例子

假如有一个需求是展示所有的文件夹、子文件夹、文件，并且不确定有多少文件夹和文件，也许是 5、10 或者
100 个文件夹。此时你会想到用递归。

简化所描述问题，我需要创建：

- the root component

- the folder component

## 数据结构

前文提到，递归总是跟随于分层组织的数据，子数据的结构与父数据结构相同

```js
const folder = {
  text: "Root Folder",
  leaf: false,
  expanded: true,
  children: [
    {
      text: "Sub Folder 1",
      leaf: false,
      expanded: false,
      children: [
        {
          text: "Sub Sub Folder 1",
          leaf: false,
          expanded: false,
          children: [
            {
              text: "SomeFile1",
              leaf: true,
            },
          ],
        },
        {
          text: "Sub Sub Folder 2",
          leaf: false,
          expanded: false,
        },
        {
          text: "SomeFile2",
          leaf: true,
        },
      ],
    },
  ],
};
```

## The Root Component

Root 组件我们文件树的开始点，它将初始渲染有所的子元素，也可以展示一些独立的信息，因为它并不是递归
自己的部分。

递归组件有一个重要的属性，姑且设为`folder`，我们将上文中描述的 folder 数据结构传给此属性，递归组件
会根据传入的数据递归的创建文件树

```html
<template>
  <ul class="folders">
    <li>所有文件</li>
    <folder :folder="folder" />
  </ul>
</template>
```

```js
import Folder from "./Folder.vue";

export default {
  name: "root",
  props: {
    folder: Object,
  },
  components: {
    Folder,
  },
};
```

## The Folder Component

这个组件负责渲染文件树，它将会展示当前文件夹信息和渲染它的子元素，文件夹是可点击的，通过点击可以
展开和收起文件夹。

```html
<template>
  <li class="folder" :class="[folder.leaf ? 'is-leaf' : 'is-folder']">
    <span @click="expand">{{ folder.text }}</span>

    <ul
      class="sub-folder"
      v-if="folder.children && folder.children.length > 0"
      v-show="folder.expanded"
    >
      <folder v-for="child in folder.children" :folder="child" />
    </ul>
    <div v-else class="folder-empty" v-show="!folder.leaf && folder.expanded">
      No Data
    </div>
  </li>
</template>
```

```js
export default {
  name: "folder",
  props: {
    folder: Object,
  },
  methods: {
    expand() {
      if (this.folder.leaf) {
        return;
      }
      this.folder.expanded = !this.folder.expanded;
    },
  },
};
```

## Example Usage

```html
<template>
  <div>
    <Root :folder="folder" />
  </div>
</template>
```

```js
import Root from "./Root.vue";

export default {
  name: "app",
  components: {
    Root,
  },
};
```
