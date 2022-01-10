# Nginx 配置通用法语

Nginx 的配置文件其实是一个普通的文件，看一个简单例子

```bash
user nobody;
worker_process  8;
error_log  var/log/nginx/error.log error;
#pid  logs/nginx.pid;
events {
  use epoll;
  worker_connections  50000;
}
http {
  include  mine.types;
  default_type  application/octet-stream;
  log_format main  '$remote_addr [$time_local] "$request"'
                   '$status $bytes_sent "$http_referer"'
                   '"$http_user_agent" "$http_x_forwarded_for"';
  access_log  logs/access.log  main buffer=32k;
}
```

## 块配置项

块配置项由一个配置块项名和一对大括号组成

```
events {
  ...
}
http {
  upstream backend {
    server  127.0.0.1:8080;
  }
  gzip on;
  server {
    location /webstatic {
      gzip off;
    }
  }
}
```

块配置项可以嵌套，内层块直接继续外层块，例如，server 块里的任意配置都是基于 http 块里的已有配置的，
当内外层块中的配置发生冲突时，究竟是以内层块还是外层块的配置为准，取决于解析这个配置项的模块。

上例中，http 模块中已经打开了`gzip on`，但其下的`location/webstatic`又把 gzip 关闭了，
最终在`/webstatic`的处理模块中，gzip 是按照 off 来处理的。

## 配置项的语法格式

在行首的是配置项名，这些配置项名必须是 Nginx 的某一个模块想要处理的，否则 Nginx 会认为配置文件出现了
非法的配置项名。

配置项名输入结束后，将以空格作为分隔符，配置项值，它可以是数字或字符串，当然也包括正则表达式，针对一个配置项，
既可以是一个值，也可以包含多个值，配置项值之间仍然由空格来分割，配置项的结尾需要加上分号

> 如果配置项值中包含语法符号，比如空格符，那么需要使用单引号或双引号括住配置项值
> 否则 Nginx 会报语法错误
> 例如
>
> ```
> log_format main '$remote_addr - $remote_user [$time_local] "$request" '
> ```

## 配置项的单位

大部分模块遵循一些通用的规定，如指定空间大小时不用每次都定义到字节、指定时间时不用精确到毫秒

指定空间大小，可以使用的单位

- K 或者 k(千字节/KB)

- M 或者 m(兆字节/MB)

例如

```
gzip     48k;
client_max_body_size  64M;
```

指定时间单位包括

ms(毫秒)，s(秒)，m(分钟)，h(小时)，d(天)，w(周，包含 7 天)，M(月，包含 30 天)，y(年，包含 365 天)

例如

```
expires    10y;
proxy_read_time  600;
client_body_timeout 2m;
```

## 在配置中使用变量

有些模块允许在配置项中使用变量，如在日志记录部分

```
log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                '$status $bytes_sent "$http_referer"'
                '"$http_user_agent" "$http_x_forwarded_for"'
```

其中`remote_addr`是一个变量，使用它的时候前面要加上`$`符号。需要注意的是，这种变量只有少数模块支持，并不是通用的。

许多模块在解析请求时都会提供多个变量，以使其他模块的配置可以即时使用，我们在学习某个模块提供的配置说明时可以关注它是否
提供变量

> 在执行`configure`命令时，我们已经把许多模块编译进 Nginx 中，但是否启用这些模块
> 一般取决于配置文件中相应的配置项。大部分模块都必须在`nginx.conf`中读取某个配置项才会在运行时启用
> 例如，只有当配置 http {...}这个配置项时，`ngx_http_module`模块才会在 Nginx 中启用，其他依赖
> `ngx_http_module`的模块也才能正常使用

## 正常运行的配置项

### 嵌入其他配置文件

语法

```
include pathfile
```

`include`配置项可以将其他配置文件嵌入到当前的`nginx.conf`文件中，它的参数既可以是绝对路径，
也可以是相对路径

```
include mime.types;
include vhost/*.conf;
```
