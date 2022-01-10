# 静态 Web 服务器

静态 Web 服务器的主要功能由`ngx_http_core_module`模块(HTTP 框架的主要成员)实现，当然，
一个完整的静态 Web 服务器还有许多功能是由其他的 HTTP 模块实现的。

本节主要讨论如何配置一个包含基本功能的静态 Web 服务器，文中会完整的说明`ngx_http_core_module`模块提供的配置项及
变量的用法。

读着应当通过简单的查询相关模块，如`ngx_http_core_module`、`ngx_http_image_filter_module`
等的配置项说明，方便地在`nginx.conf`配置文件中加入新的配置项。从而实现更多的 Web 服务器功能

一个典型的静态服务器会包含多个`server`块和`location`块

```
http {
  include       mime.types;
  default_type  application/octet-stream;
  sendfile      on;
  keepalive_timeout 65;

  server {
    listen       8080;
    server_name  localhost;

    location / {
      root   html;
      index  index.html index.htm
    }

    error_page 500 502 503 504  /50x.html;

    location = /50x.html {
      root  html;
    }
  }

  server {}

  include servers/*;
}
```

所有 HTTP 配置项都必须直属于 http 块、server 块、location 块、upstream 块或
if 块等，在描述每个配置项的功能时，会说明它可以在上述的哪个块中存在，因为有些配置
项可以任意地出现在某一块中，而有些配置只能出现在特定的块中。

Nginx 为配置一个完整的静态 Web 服务器提供了非常多的功能，下面会把这些配置项分为以下
8 类进行详述

- 虚拟主机与请求的分发

- 文件路径的定义

- 内存及磁盘资源的分配

- 网络连接的设置

- MIME 类型的设置

- 对客户端请求的限制

- 文件操作的优化

- 对客户端请求的特殊处理

这种划分只是为了帮助大家从功能上理解这些配置项

这之后会列出`ngx_http_core_module`模块提供的变量，以及简单说明它们的意义
