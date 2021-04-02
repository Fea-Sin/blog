## localStorage 和 sessionStorage 区别

`localStorage`和`sessionStorage`一样都是用来存储客户端临时信息的对象

他们均只能存储字符串类型的对象（虽然规范中可以存储其他原生类型的对象，但是目前为止没有浏览器对其进行实现）

`localStorage`生命周期是永久，这意味着除非用户主动在浏览器上清除`localStorage`信息，否则这些信息将永远
存在

`sessionStorage`生命周期为当前窗口或标签页（当前会话），一旦窗口或标签页被关闭了，那么所有通过`sessionStorage`
存储的数据也就被清空了

### sessionStorage

`sessionStorage`是 HTML5 新增的一个会话存储对象，用于临时保存同一窗口（或标签页）的数据，在关闭窗口或标签之后将会
删除这些数据

### 使用

设置存储

`setItem(key, value)`

```
localStorage.setItem("key", "value")
sessionStorage.setItem("site", "js8.cn")
```

获取存储

`getItem(key)`

```
let value = localStorage.getItem("key")
let site = sessionStorage.getItem("site")

// 滚动时保存滚动位置
$(window).scroll(function() {
  if ($(document).scrollTop() != 0) {
    sessionStorage.setItem("offsetTop", $(window).scrollTop()) // 保存滚动位置
  }
})

// 刷新时，取出滚动到上次保存位置
window.onload = function() {
  var offset = sessionStorage.getItem("offsetTop")
  $(document).scrollTop(offset)
}
```

### 存储信息

不同浏览器无法共享`localStorage`和`sessionStorage`中的信息。相同浏览器的不同页面间
可以共享同源的`localStorage`，但是不同页面或标签页面无法共享`sessionStorage`的信息。

> 同源的判断，以下三方面必须相同
>
> 协议相同
> 主机(域名)相同
> 端口相同

`localStorage`和`sessionStorage`使用相同的 API

删除存储的信息

```
localStorage.removeItem("key") // 删除名称为key的信息
localStorage.clear() // 清空localStorage中所有信息
```

通过`getItem`获取到的信息均为实际存储的副本，操作返回的值无法对实际存储的值产生副作用

`localStorage`和`sessionStorage`中键值对总是以字符串的形式存储，这意味着数值类型、复杂数据类型
会自动转化为字符串类型

### localStorage、sessionStorage 和 cookie 的异同

| 特性         | cookie                                                                             | localStorage                           | sessionStorage                               |
| ------------ | ---------------------------------------------------------------------------------- | -------------------------------------- | -------------------------------------------- |
| 数据的生命期 | 一般由服务器生成，可设置失效时间。如果在浏览器端生成 cookie 默认是关闭浏览器后失效 | 除非被清除，否则永久保存               | 仅在当前会话下有效，关闭页面或浏览器后被清除 |
| 存放数据大小 | 4k 左右                                                                            | 5MB                                    | 5Mb                                          |
| 与服务器通信 | 每次都会携带在 http header 中，如果使用 cookie 保存过多数据会带来性能问题          | 仅在客户端中保存，不参与和服务器的通信 | 仅在客户端中保存，不参与和服务器的通信       |

### 应用场景

因为考虑到每个 HTTP 请求都会带着 cookie 的信息（cookie 也是有域/源的概念），比较常见的一个应用
场景是判断用户是否登陆。当用户登陆后，服务器端会在他登陆时往 cookit 中插入一段加密的唯一辨识码，
下次只要读取这个值就可以判断当前用户是否登陆了。

HTML5 游戏通常会产生一些本地数据，localStorage 会非常适用这种场景

如果遇到一些内容特别多的表单为了优化用户体验，可以把表单拆分成多个子页面，这时候
sessionStorage 作用就发挥出来了（刷新页面，可以存留已填写的数据）。

### 安全考虑

需要注意的是，不是什么数据都适合放在 cookie、localStorage 和 sessionStorage 中，
因为只要打开控制台，你就可以随意修改它们的值。如果你的网站有 XSS 风险，他们就可以查看 cookie、
localStorage、sessionStorage 的所有信息。
