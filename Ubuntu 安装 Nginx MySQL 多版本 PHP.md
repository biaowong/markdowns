# Ubuntu 安装 Nginx MySQL 多版本 PHP

## 升级系统

```shell
sudo apt update
```



## MySQL 安装

```shell
# 安装 MySQL 服务
sudo apt install mysql-server
# 初始化密码及一下安全设置，密码强度建议选 2 ，其它项全选 y
sudo mysql_secure_installation
# 访问 MySQL，不加 sudo 会报错：ERROR 1698 (28000): Access denied for user 'root'@'localhost'
sudo mysql -uroot -p

# 操作命令
sudo systemctl start mysql.service 		# 开启
sudo systemctl restart mysql.service	# 重启
sudo systemctl stop mysql.service			# 停止
sudo systemctl enable mysql.service 	# 开机启动
```

## Nginx 安装

```shell
sudo apt install nginx

# 访问 Nginx 格式：http://ip_address
http://192.168.0.21/

# 操作命令
sudo systemctl start nginx.service 		# 开启
sudo systemctl restart nginx.service	# 重启
sudo systemctl stop nginx.service			# 停止
sudo systemctl enable nginx.service 	# 开机启动
```

## 启用软件库，添加 PHP 版本

```shell
# Ondřej Surý 是一名 Debian 开发人员，他维护着一个包含多个 PHP 版本的仓库。
# 启用 PPA 仓库可以通过运行以下命令：
sudo apt install software-properties-common
sudo add-apt-repository ppa:ondrej/php
```

## PHP 8.1 安装

```shell
# 依据个人业务及框架需求安装扩展，本扩展参考：Laravel 9
wget https://www.php.net/distributions/php-8.1.5.tar.gz

sudo apt install php8.1 php8.1-fpm php8.1-mysql php8.1-redis php8.1-bcmath php8.1-curl php8.1-gd 
```



