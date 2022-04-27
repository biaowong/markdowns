# Ubuntu 安装 Nginx MySQL Redis 多版本 PHP

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

## Redis 安装

```shell
sudo apt install redis-server
```

## PHP 环境依赖安装

```shell
sudo apt install -y pkg-config build-essential autoconf bison re2c libxml2-dev libsqlite3-dev libcurl4-gnutls-dev libjpeg-dev libpng-dev libmcrypt-dev  libreadline6-dev libfreetype6-dev libtidy-dev libtool valgrind openssl libssl-dev libzip-dev libwebp-dev libxpm-dev libkrb5-dev libbz2-dev libonig-dev libxslt-dev
```

## PHP 8.1 编译安装

```shell
cd /usr/local/src
# 依据个人业务及框架需求安装扩展，本扩展参考：Laravel 9
sudo wget https://www.php.net/distributions/php-8.1.5.tar.gz
sudo tar zxvf php-8.1.5.tar.gz
cd php-8.1.5

sudo ./configure \
--prefix=/usr/local/php81 \
--exec-prefix=/usr/local/php81 \
--bindir=/usr/local/php81/bin \
--sbindir=/usr/local/php81/sbin \
--includedir=/usr/local/php81/include \
--libdir=/usr/local/php81/lib/php \
--mandir=/usr/local/php81/php/man \
--with-config-file-path=/usr/local/php81/etc \
--with-fpm-user=www-data \
--with-fpm-group=www-data \
--with-curl \
--with-freetype \
--with-mysqli \
--with-mysql-sock=/tmp/mysql.sock \
--with-pdo-mysql \
--with-kerberos \
--with-libdir=lib64 \
--with-openssl \
--with-pear \
--with-gettext \
--with-jpeg \
--with-libxml \
--with-pdo-sqlite \
--with-mhash \
--with-ldap-sasl \
--with-xsl \
--with-zlib \
--with-zip \
--with-bz2 \
--with-iconv \
--enable-fpm \
--enable-pdo \
--enable-gd \
--enable-xml \
--enable-bcmath \
--enable-shmop \
--enable-sysvsem \
--enable-opcache \
--enable-mbregex \
--enable-mbstring \
--enable-ftp \
--enable-pcntl \
--enable-exif \
--enable-sockets \
--enable-soap \
--enable-session \
--enable-ctype \
--enable-mysqlnd \
--enable-intl \
--enable-calendar \
--enable-static \
--enable-mysqlnd

sudo make -j 8 # 数字 8 是 CPU 的核数
sudo make install

# 复制 php 配置文件到安装目录
sudo cp php.ini-production /usr/local/php81/etc/php.ini
sudo cp /usr/local/php81/etc/php-fpm.d/www.conf.default /usr/local/php81/etc/php-fpm.d/www.conf
sudo cp /usr/local/php81/etc/php-fpm.conf.default /usr/local/php81/etc/php-fpm.conf

# 复制启动脚本并给予可执行权
sudo cp /usr/local/src/php-8.1.5/sapi/fpm/init.d.php-fpm /etc/init.d/php8.1-fpm
sudo chmod +x /etc/init.d/php8.1-fpm

# 关联 php 配置文件
sudo mkdir /etc/php
sudo ln -s /usr/local/php81/etc/php.ini /etc/php/php81.ini
```

## PHP 7.4 编译安装

```shell
cd /usr/local/src
sudo wget https://www.php.net/distributions/php-7.4.29.tar.gz
sudo tar zxvf php-7.4.29.tar.gz
cd php-7.4.29

sudo ./configure \
--prefix=/usr/local/php74 \
--exec-prefix=/usr/local/php74 \
--bindir=/usr/local/php74/bin \
--sbindir=/usr/local/php74/sbin \
--includedir=/usr/local/php74/include \
--libdir=/usr/local/php74/lib/php \
--mandir=/usr/local/php74/php/man \
--with-config-file-path=/usr/local/php74/etc \
--with-fpm-user=www-data \
--with-fpm-group=www-data \
--with-curl \
--with-freetype \
--with-mysqli \
--with-mysql-sock=/tmp/mysql.sock \
--with-pdo-mysql \
--with-kerberos \
--with-xpm-dir=/usr/lib64 \
--with-libdir=lib64 \
--with-openssl \
--with-mcrypt \
--with-pear \
--with-gettext \
--with-jpeg \
--with-png \
--with-libxml \
--with-pdo-sqlite \
--with-mhash \
--with-ldap-sasl \
--with-xsl \
--with-zlib \
--with-zip \
--with-bz2 \
--with-iconv \
--enable-fpm \
--enable-pdo \
--enable-gd \
--enable-xml \
--enable-bcmath \
--enable-shmop \
--enable-sysvsem \
--enable-inline-optimization \
--enable-opcache \
--enable-mbregex \
--enable-mbstring \
--enable-ftp \
--enable-pcntl \
--enable-exif \
--enable-sockets \
--enable-soap \
--enable-session \
--enable-ctype \
--enable-mysqlnd \
--enable-intl \
--enable-calendar \
--enable-static \
--enable-mysqlnd

sudo make -j 8 # 数字 8 是 CPU 的核数
sudo make install

# 复制 php 配置文件到安装目录
sudo cp php.ini-production /usr/local/php74/etc/php.ini
sudo cp /usr/local/php74/etc/php-fpm.d/www.conf.default /usr/local/php74/etc/php-fpm.d/www.conf
sudo cp /usr/local/php74/etc/php-fpm.conf.default /usr/local/php74/etc/php-fpm.conf

# 多版本 php 共存，需要修改监听的端口号 9000 改为 9001
sudo vim /usr/local/php74/etc/php-fpm.d/www.conf
listen = 127.0.0.1:9001 

# 复制启动脚本并给予可执行权
sudo cp /usr/local/src/php-7.4.29/sapi/fpm/init.d.php-fpm /etc/init.d/php7.4-fpm
sudo chmod +x /etc/init.d/php7.4-fpm

# 关联 php 配置文件
sudo ln -s /usr/local/php74/etc/php.ini /etc/php/php74.ini
```

## PHP 5.6 编译安装

```shell
cd /usr/local/src
sudo wget https://www.php.net/distributions/php-5.6.40.tar.gz
sudo tar zxvf php-5.6.40.tar.gz
cd php-5.6.40

sudo ./configure \
--prefix=/usr/local/php56 \
--exec-prefix=/usr/local/php56 \
--bindir=/usr/local/php56/bin \
--sbindir=/usr/local/php56/sbin \
--includedir=/usr/local/php56/include \
--libdir=/usr/local/php56/lib/php \
--mandir=/usr/local/php56/php/man \
--with-config-file-path=/usr/local/php56/etc \
--with-fpm-user=www-data \
--with-fpm-group=www-data \
--with-curl \
--with-freetype \
--with-mysqli \
--with-mysql-sock=/tmp/mysql.sock \
--with-pdo-mysql \
--with-kerberos \
--with-xpm-dir=/usr/lib64 \
--with-libdir=lib64 \
--with-openssl \
--with-mcrypt \
--with-pear \
--with-gettext \
--with-jpeg \
--with-png \
--with-libxml \
--with-pdo-sqlite \
--with-mhash \
--with-ldap-sasl \
--with-xsl \
--with-zlib \
--with-zip \
--with-bz2 \
--with-iconv \
--enable-fpm \
--enable-pdo \
--enable-gd \
--enable-xml \
--enable-bcmath \
--enable-shmop \
--enable-sysvsem \
--enable-inline-optimization \
--enable-opcache \
--enable-mbregex \
--enable-mbstring \
--enable-ftp \
--enable-pcntl \
--enable-exif \
--enable-sockets \
--enable-soap \
--enable-session \
--enable-ctype \
--enable-mysqlnd \
--enable-intl \
--enable-calendar \
--enable-static \
--enable-mysqlnd

sudo make -j 8 # 数字 8 是 CPU 的核数
sudo make install

# 复制 php 配置文件到安装目录
sudo cp php.ini-production /usr/local/php56/etc/php.ini
sudo cp /usr/local/php56/etc/php-fpm.d/www.conf.default /usr/local/php56/etc/php-fpm.d/www.conf
sudo cp /usr/local/php56/etc/php-fpm.conf.default /usr/local/php56/etc/php-fpm.conf

# 多版本 php 共存，需要修改监听的端口号 9000 改为 9002
sudo vim /usr/local/php74/etc/php-fpm.d/www.conf
listen = 127.0.0.1:9002

# 复制启动脚本并给予可执行权
sudo cp /usr/local/src/php-5.6.40/sapi/fpm/init.d.php-fpm /etc/init.d/php5.6-fpm
sudo chmod +x /etc/init.d/php5.6-fpm

# 关联 php 配置文件
sudo ln -s /usr/local/php56/etc/php.ini /etc/php/php56.ini
```

## PHP 环境变量

```shell
# 添加 php 环境变量，依据项目需求切换对应的 php 版本
vim ~/.profile
export PATH=/usr/local/php81/bin:$PATH
# export PATH=/usr/local/php74/bin:$PATH
# export PATH=/usr/local/php56/bin:$PATH

source ~/.profile
```

## PHP-Redis 安装

```shell
cd /usr/local/src
sudo wget http://pecl.php.net/get/redis-5.3.7.tgz
sudo tar xvf redis-5.3.7.tgz
cd redis-5.3.7
# 编译 php 8.1 版本的 redis 扩展，其它版本编译安装参考此例操作
sudo /usr/local/php81/bin/phpize
sudo ./configure --with-php-config=/usr/local/php81/bin/php-config
sudo make -j 8
sudo make install

sudo vim /etc/php/php81.ini
# 在最下方添加 redis 扩展
extension=redis
```

## 开机启动配置

**脚本编写**

```shell
sudo vim /etc/init.d/auto-start
### BEGIN INIT INFO
# Provides:          auto-start
# Required-Start:    $remote_fs $syslog
# Required-Stop:     $remote_fs $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: starts auto-start
# Description:       starts the PHP, Nginx, MySQL，Redis process
### END INIT INFO

NAME=auto-start

if []


if [ "$1" != "start" ] && [ "$1" != "stop" ] && [ "$1" != "restart" ]
then
        echo "Usage: $NAME {start|stop|restart}" >&2
        exit 3
fi

# MySQL 服务
/etc/init.d/mysql $1
# Redis 服务
/etc/init.d/redis-server $1
# PHP5.6 服务
#/etc/init.d/php5.6-fpm restart
# PHP7.4 服务
#/etc/init.d/php7.4-fpm restart
# PHP8.1 服务
/etc/init.d/php8.1-fpm $1
# Nginx 服务
/etc/init.d/nginx $1

```

**自启动脚本编写**

```shell
sudo vim /etc/systemd/system/auto-start.service
[Unit]
Description=Start the auto-start server
Documentation=by:hiwb@tutanota.com, man:auto-start(1)
After=network.target

[Service]
Type=forking
Restart=always
TimeoutSec=0
ExecStart=/etc/init.d/auto-start start
ExecRestart=/etc/init.d/auto-start restart
ExecStop=/etc/init.d/auto-start stop

[Install]
WantedBy=multi-user.target
Alias=auto-start.service

sudo chmod +x /etc/systemd/system/auto-start.service

sudo systemctl daemon-reload
sudo systemctl start auto-start.service
sudo systemctl enable auto-start.service
```

