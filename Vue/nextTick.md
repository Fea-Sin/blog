# Vue.nextTick([callback, context])

## 参数

- {Function} [callback]
- {Object} [context]

## 用法

在下次 DOM 更新循环结束之后，执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM

```js
vm.msg = "hello";

// DOM还没更新
Vue.nextTick(function () {
  // DOM更新了
});
```
