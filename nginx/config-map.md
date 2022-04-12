接口部署映射配置

```
location ~^/app/(.*)$ {
    proxy_pass       http://192.169.154.102:8069/some_dir/$1;
}
```

test.com/app/request/xxxxx => http://192.168.154.102:9999/some_dir/request/xxxxx
