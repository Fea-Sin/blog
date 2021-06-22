## History API

浏览器 window 通过`history`对象提供了对浏览器的会话历史的访问。它暴露了很多有用的方法和属性，
允许你在用户浏览历史中向前和向后跳转，从`HTML5`开始提供了对`history`栈中内容的操作

### 向前和向后跳转

```js
window.history.back();
```

这和用户点击浏览器回退按钮的效果相同

你可以向前跳转（如同用户点击了前进按钮）

```js
window.history.forward();
```

跳转到 history 中的指定的一个点，你可以用`go()`方法载入到会话历史中的某一特定页面，通过与当前页面相对
位置来标志，当前页面的相对位置标志为 0

向后移动一个页面

```js
window.history.go(-1);
```

向前移动两个页面

```js
window.history.go(2);
```

可以通过查看长度属性的值来确定历史堆栈中页面的数量

```js
let numberOfEntries = window.history.length;
```

### 添加和修改历史记录中的条目

HTML5 引入了`history.pushState()`和`history.replaceState()`方法，它们分别可以添加和修改历史
记录条目。这些方法通常与`window.onpopstate`配合使用

使用`history.pushState()`可以改变`referrer`，他在用户发送`XMLHttpRequest`请求时在 HTTP 头部使用，
改变 state 后创建的`XMLHttpRequest`对象的 referrer 都会被改变。因为 referrer 是标识创建`XMLHttpRequest`
对象时所代表的 window 对象中的 document 的 URL。

### 动机

history API 最重要的是它不会刷新页面，就像改变 hash 一样（例如: <a href="#target">link</a>）

`window.location`总是会刷新页面

### API

```js
history.pushState(data, title, url);
```

- 第一个参数，路由导航时可以存储一些数据，当导航事件发生可以获取使用

- 第二参数 title，浏览器都默认忽视

- 第三个参数 url，我们想要显示在浏览器地址栏里的 url 地址

### 实例

```js
window.onpopstate = function (event) {
  console.log(`location: ${document.location}, state: ${event.state}`);
};

history.pushState({ page: 1 }, "title 1", "?page=1");
history.pushState({ page: 2 }, "title 2", "?page=2");
history.replaceState({ page: 3 }, "title 3", "?page=3");

history.back(); // localtion: http://example.com/example.html?page=1, state: {"page":1}
history.back(); // location: http://example.com/example.html, state: null
history.go(2); //   location: http://example.com/example.html?page=3, state: {"page":3}
```

> 注意
>
> 第三个参数 url 必须是同源的，否则会抛出错误
