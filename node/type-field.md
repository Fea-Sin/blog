# Node.js Package type Field

- type: `string`

`type`字段确定 Node.js 包的 module format，包中所有`.js`的 module format 以最近的
`package.json`确定

## 被判定为`ES module`的情况

```json
// package.json
{
  "type": "module"
}
```

## 被判定为`CommonJS`的情况

**第一种情况**

```json
// package.json
{
  "type": "commonjs"
}
```

**第二种情况**

在最近的`package.json`文件中不存在`type`字段

**第三种情况**

沿文件夹找到`root`路径都没有找到`package`文件

> 注意
>
> 不论`type`字段是啥值，`.mjs`文件都将被视为`ES module`，`cjs`文件将被视为`CommonJS`
