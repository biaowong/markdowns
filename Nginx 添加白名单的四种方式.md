# Nginx 添加白名单的四种方式

## 1. 防火墙白名单
添加防火墙白名单，针对 Nginx 域名配置所启用的端口（比如80端口）在 ufw 里做白名单，比如只允许 211.158.188.11 访问。但是这样就把 Nginx 的所有80端口的域名访问都做了限制，范围比较大，不够灵活。如下：
```shell
# 只允许 211.158.188.11、211.158.188.12、211.158.188.13 访问 80 端口
sudo ufw allow proto tcp from 211.158.188.11/24 to any port 80
sudo ufw allow proto tcp from 211.158.188.12/24 to any port 80
sudo ufw allow proto tcp from 211.158.188.13/24 to any port 80
```

## 2. 判断 $remote_addr

针对 Nginx 下的某一个域名进行访问的白名单限制，那么可以在 Nginx 的配置文件里进行设置，利用 $remote_addr 参数进行访问的分发限制，如下：

```nginx
server {
    listen 80;
    server_name hostname.com;
    root /var/www/project_floder;
    
    # 只允许 211.158.188.11、211.158.188.12、211.158.188.13 访问
    if ($remote_addr !~ ^(211.158.188.11|211.158.188.12|211.158.188.13|127.0.0.1)) {
        rewrite ^.*$ /index.php last;
    }
    
    ...
}
```

## 3. 判断 $http_x_forwarded_for

使用 $http_x_forwarded_for 参数进行访问的分发限制，如下：

```nginx
server {
    listen 80;
    server_name hostname.com;
    root /var/www/project_floder;
    
    # 只允许 211.158.188.11、211.158.188.12、211.158.188.13 访问
    if ($http_x_forwarded_for !~ ^(211.158.188.11|211.158.188.12|211.158.188.13|127.0.0.1)) {
        rewrite ^.*$ /index.php last;
    }
    
    ...
}
```

## 4. 利用 allow 及 deny 控制

利用 Nginx 的 allow、deny 参数进行访问限制，如下：

```nginx
server {
    listen 80;
    server_name hostname.com;
    root /var/www/project_floder;
    
    # 只允许 211.158.188.11、211.158.188.12、211.158.188.13 访问
    allow 211.158.188.11;
    allow 211.158.188.12;
    allow 211.158.188.13;
    allow 127.0.0.1;
    deny all;
    
    ...
}
```

