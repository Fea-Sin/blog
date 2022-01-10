# install

## macOS

```bash
brew install nginx
```

Docroot

```bash
/usr/local/var/www
```

配置位置

```bash
/usr/local/etc/nginx/nginx.conf
```

## 安装完成后启动 nginx

```bash
sudo nginx
```

## 停止 nginx

```bash
sudo nginx -s stop

# or

sudo pkill nginx
```

## 修改配置后，惹重启

```
sudo nginx -s reload
```

## mac nginx log 位置

当 nginx 出现问题时，可以查看报错原因

```
/usr/local/var/log/nginx
```
