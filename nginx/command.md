# NGINX COMMAND IN LINUX

**测试配置是否正确**

```bash
sudo nginx -t
# nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
# nginx: configuration file /etc/nginx/nginx.conf test is successful
```

**优雅的重启**

```
sudo systemctl reload nginx
```

**Fully restart**

```
sudo systemctl restart nginx
```

**查看 nginx 状态**

```
sudo systemctl status nginx
```

[refer](https://linuxconfig.org/how-to-restart-nginx-on-linux)
