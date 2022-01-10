# 文件路径的定义

## 以 root 方式设置资源路径

> 配置块: `http`, `server`, `location`, `if`

语法

```bash
root path;

# 默认值 root  html;
```

例如，定义资源文件相对于 HTTP 请求的根目录

```
location /download/ {
  root  optwebhtml;
}
```

在上面的配置中，如果一个请求的 URI 是`/download/index/test.html`，那么 web 服务器将会
返回服务器上`/optwebhtml/download/index/test.html`文件的内容

## 以 alias 方式设置资源路径

> 配置块: `location`

语法

```
alias path;
```

alias 也是用来设置文件资源路径的，它与 root 的不同点主要在于如何解读紧跟 location 后的
uri 参数，这将会致使 alias 与 root 以不同的方式将用户请求映射到真正的磁盘文件上

例如，如果有一个请求的 URI 是`/conf/nginx.conf`，如果用 alias 来进行设置的话，方式如下

```
location conf {
  alias usr/local/nginx/conf/;
}
```

如果用 root 来设置，方式如下

```
location conf {
  root usr/local/nginx/
}
```

## root 方式与 alias 方式的区别

**alias 方式**

`/conf/nginx.conf`的请求，以`alias path`方式映射为`path/nginx.conf`

**root 方式**

`/conf/nginx.conf`的请求，以`root path`方式映射为`path/conf/nginx.conf`

这也是 root 可以放置到`http`,`server`,`location`,`if`块中，而 alias 只能放置到`location`块中
的原因

**alias 后面还可以添加正则表达式**

```
location ~ ^/test/(\w+)\.(\w+)$ {
  alias usr/local/nginx/$2/$1.$2;
}
```

> `\w` 匹配包括下划线的任何单词字符，等价于`[A-Za-z0-9_]`

在请求`/test/nginx.conf`时，Nginx 会返回`usr/local/nginx/conf/nginx.conf`

## 访问首页

> 配置块: `http`, `server`, `location`

语法

```bash
index  file;

# 默认值 index  index.html index.htm;
```

当访问站点的 URI 是`/`，这时一般是返回网站首页，而这与 root 和 alias 都不同，这里用`ngx_http_index_module`
模块提供的`index`配置实现，index 后可以跟多个参数，Nginx 会按照顺序来访问这些文件

```
location {
  root path
  index index.html htmlindex.php index.php
}
```

收到`/`请求后，Nginx 首先会尝试访问`path/index.php`文件，如果可以访问，就直接返回文件内容结束请求，
否则再试图返回`path/htmlindex.php`文件的内容，以此类推
