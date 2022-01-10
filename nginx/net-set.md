# 网络连接的设置

- `413` Request entity too large

## 对某些浏览器禁用`keepalive`功能

> 配置块: `http`, `server`, `location`

HTTP 请求中的`keepalive`功能是为了让多个请求复用一个 HTTP 长连接，这个功能对服务器
的性能提高是很有帮助的，但有些浏览器如 IE6 和 Safari，它们对于使用`keepalive`功能的
POST 请求处理有功能性问题。因此，针对 IE6 级早期的 Safari 浏览器是禁用`keepalive`
功能的

一个 keepalive 连接在闲置超过默认 75 秒后，服务器和浏览器都会去关闭这个连接

一个 keepalive 连接上默认最多只能发送 100 个请求

## MIME 类型的设置

MIME type 与文件扩展的映射

> 配置块: `http`, `server`, `location`

例如

```
type {
  text/html  html;
  text/html  conf;
  text/gif   gif;
  text/jpeg  jpg;
}
```

### 默认 MIME type

当找不到相应的 MIME type 与文件扩展名之间的映射，使用默认的 MIME type 作为
HTTP header 中的`content-type`

例如

```
default_type  text/plain
```

## 对客户端请求的限制

Nginx 通过`limit_except`后面指定的方法名来限制用户请求，方法名可取值包括
`GET`, `HEAD`, `POST`, `POST`, `PUT`, `DELETE`, `MKCOL`, `COPY`,
`MOVE`, `OPTIONS`, `PROPFIND`, `PROPPATCH`, `LOCK`, `UNLOCK`,`PATCH`

> 配置块: `location`

例如

```
limit_except GET {
  allow 192.168.1.0/32;
  deny all;
}
```

> 允许 GET 方法就意味着也允许 HEAD 方法，上面这段代码表示的是禁止 GET 方法和 HEAD 方法
> 但其他 HTTP 方法是允许的

### 对`if-modified-since`头部的处理策略

> 配置块: `http`, `server`, `location`

出于性能考虑，web 浏览器一般会在客户端本地缓存一些文件，并存储当时获取的时间。
这样，下次向 web 服务器获取缓存过的资源时，就可以用`if-modified-since`头部
把上次获取的时间捎带上，则`if-modified-since`将根据后面的参数决定如何处理
`if-modified-since`头部

相关参数说明

- `off` 表示忽略用户请求中的`if-modified-since`头部，这时，如果获取一个文件，
  那么会正常地返回文件内容，HTTP 响应码通常是`200`

- `exact` 将`if-modified-since`头部包含的时间与将要返回的文件上次修改的时间
  做精确比较，如果没有匹配上，则返回 200 和文件的实际内容，如果匹配上，则表示
  浏览器缓存的文件内容已经是最新的了，没有必要在返回文件从而浪费时间与带宽了，这时
  返回`304`Not Modified，浏览器收到后会直接读取自己的本地缓存

- `before` 是比`exact`更宽松的比较，只要文件的上次修改时间等于或者早于用户请求中
  的`if-modified-since`头部的时间，就会向客户端返回 304
