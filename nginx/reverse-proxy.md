# 用 HTTP Proxy module 配置一个反向代理服务器

反向代理(reverse proxy)方式是指用代理服务器来接受 Internet 上的连接请求，然后将
请求转发给内部网络中的上游服务器，并将从上游服务器上得到的结果返回给 Internet 上请求
连接的客户端

此时，代理服务器对外的表现就是一个 web 服务器，充当反向代理服务器也是 Nginx 的一种常见用法，
反向代理服务器必须能处理大量并发请求

由于 Nginx 具有`强悍`的高并发高负载能力，因此一般会作为前端的服务器直接向客户端提供静态
文件服务。但也有一些复杂、多变的业务不适合放到 Nginx 服务器上，这时会用到`Apache`, `Tomcat`
等服务器来处理

## 负载均衡的基本配置

作为代理服务器，一般都需要向上游服务器的集群转发请求，这里的负载均衡是指选择一种策略，尽量
把请求平均地分布到每一台上游服务器上

**upstream 块**

> 配置块: `http`

upstream 块定义了一个上游服务器的集群，便于反向代理中的`proxy_pass`使用

```
upstream backend {
  server backend1.example.com;
  server backend2.example.com;
  server backend3.example.com;
}
server {
  location / {
    proxy_pass  http://backend;
  }
}
```

server 配置项指定了一台上游服务器的名字，这个名字可以是域名、IP 地址端口、UNIX 句柄

```
upstream backend {
  server backend1.example.com;
  server 127.0.0.1:8080;
  server unix:/tmp/backend3;
}
```
