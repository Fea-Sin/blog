# 虚拟主机与请求分发

由于 IP 地址的数量有限，因此经常存在多个主机域名对应同一个 IP 地址的情况，这在
`nginx.conf`中就可以按照`server_name`(对应用户请求中的主机域名)并通过 server 块
来定义虚拟主机

每个 server 块就是一个虚拟主机，它只处理与之相对应的主机域名请求。

这样一台服务器上的 Nginx 就能以不同的方式处理访问不同主机域名的 HTTP 请求了

## 监听端口

> 配置块: `server`

语法

```
listen address:port [options]
```

实例

```bash
listen 127.0.0.1:8000;

# 不加端口时，默认监听80端口
listen 127.0.0.1;
```

**如果使用 IPv6 地址**

```
listen [::]:8000;

listen [fe80::1];
```

在地址和端口后，还可以加上其他参数

```
listen 443 default_server ssl;

listen 127.0.0.1 default_server accept_filter=dataready backlog=1024
```

- `default_server`

将所在的 server 块作为整个 Web 服务的默认 server 块。如果没有设置这个参数，那么将会在
`nginx.conf`中找到的第一个 server 块作为默认 server 块

为什么需要默认虚拟主机呢，当一个请求无法匹配配置文件中的所有主机域名时，就会选用默认的虚拟主机

- `ssl`

在当前监听的端口上建立的连接必须基于 SSL 协议

## 主机名称

> 配置块: `server`

语法

```bash
server_name  name;

# server_name 后可以跟多个主机名称，如`server_name  www.testweb.com download.testweb.com`
```

在开始处理一个 HTTP 请求时，Nginx 会取出`header`头中的`host`，与每个 server 中的`server_name`
进行匹配，以此决定到底由哪一个 server 块来处理这个请求。

有可能一个`host`与多个 server 块中的`server_name`都匹配，这时就会根据匹配优先级来选择实际处理的
server 块。

`server_name` 与 `host` 匹配优先级

- 首先选择所有字符串完全匹配的 server_name，例如`www.testweb.com`

- 其次选择通配符在前面的 server_name，如`*.testweb.com`

- 再次选择通配符在后面的 server_name，如`www.testweb.*`

- 最后选择使用正则表达式才匹配的 server_name，如`~ ^\.testweb\.com$`

如果`host`与所有的 `server_name` 都不匹配，这时将会按下列顺序选择处理

- 优先选择在`listen`配置项后加入`default_server/default`的 server 块

- 找到匹配`listen`端口的第一个 server 块

> Nginx 正是使用`server_name`配置项针对特定`host`域名的请求提供不同的服务
> 以此实现虚拟主机功能

## location

> 配置块: `server`

语法

```
location =|~|~*|^~|@/uri/{...}
```

location 会尝试根据用户请求中的 URI 来匹配上面的`uri`表达式，如果可以匹配，就选择 location {}
块中的配置来处理用户请求

- `=` 表示把 URI 作为字符串，以便与参数中的`uri`做完全匹配

```bash
location = / {
  # 只有当用户请求是`/`时，才会使用该配置
  ...
}
```

- `~` 表示匹配 URI 时是字母大小写敏感的

- `~*` 表示匹配 URI 时忽略字母大小写问题

- `^~` 表示匹配 URI 时只需要其前半部分与`uri`参数匹配即可

```bash
location ^~/images {
  # 以 images 开始的请求都会匹配上
  ...
}
```

- `@` 表示仅用于 Nginx 服务内部请求之间的重定向，带有`@`的 location 不直接处理用户请求

当然，在`uri`参数里是可以用正则表达式，例如

```bash
location ~* \.(gif|jpg|jpeg)$ {
  # 匹配以 .gif .jpg .jpeg 结尾的请求
}
```

注意，location 是有顺序的，当一个请求有可能匹配多个 location 时，实际上这个请求会被
第一个 location 处理。

在以上各种匹配方式中，都只能表达为`如果匹配...则...`，如果需要表达`如果不匹配...则...`很难直接做到。

有一种解决方法是在最后一个 location 中使用`/`作为参数，他会匹配所有的 HTTP 请求，这样就可以
表示如果不能匹配所有前面的所有 location，则由`/`这个 location 处理

```bash
location / {
  # 可以匹配所有请求
}
```
