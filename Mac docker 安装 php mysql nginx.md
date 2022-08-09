# Mac docker 安装 php mysql nginx


## 一、Docker 部署

### 1. 下载镜像
``` bash
# pull images 镜像
docker pull ubuntu

# 查看 images
docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
ubuntu       latest    ba6acccedd29   2 months ago   72.8MB

# 创建容器并运行
docker run -i -t --name ubuntu-dev ubuntu bash
```

参数解释：
| 参数   | 值         | 含义                       |
| ------ | ---------- | -------------------------- |
| -i     | 无         | 可以输入进行交互           |
| -t     | 无         | 终端交互                   |
| –name  | ubuntu-dev | 指定容器名称为  ubuntu-dev |
| ubuntu | 无         | 指定使用镜像               |
| bash   | 无         | 指定容器启动使用的应用     |

### 2. 配置 Ubuntu
升级系统库
``` bash
apt-get update
# 安装 vim
apt-get install vim
# 安装 openssh-server
apt-get install openssh-server
```

配置 sshd
需要更改一下 sshd 的默认配置，编辑文件 /etc/ssh/sshd_config ，大概从 29 行开始主要更改三处，更改后内容如下：
``` bash?linenums
PermitRootLogin yes # 可以登录 root 用户
PubkeyAuthentication yes # 可以使用 ssh 公钥许可
AuthorizedKeysFile ~/.ssh/authorized_keys # 公钥信息保存到文件 ~/.ssh/authorized_keys 中

# 重启 sshd
/etc/init.d/ssh restart
```

添加主机的 ssh 公钥
这里的主机指的就是 macOS，保证此时还是在 ubuntu 容器中。
1. 在 HOME 目录下创建 .ssh 目录：mkdir ~/.ssh
2. 新建文件 ~/.ssh/authorized_keys ：touch ~/.ssh/authorized_keys
3. 新开一个 macOS 下的终端窗口，进入 ~/.ssh 执行 ssh-keygen 一路回车。执行命令 cat ~/.ssh/id_rsa.pub，复制打印的一行公钥信息
4. 回到 ubuntu 容器中，将第 3 步复制的公钥粘贴到 ~/.ssh/authorized_keys 中保存。

### 3. 提交修改到镜像

查看刚刚操作的容器信息，执行命令 docker ps -a ，可以看到 mineos 的状态已经是退出了，主要关注 mineos 的 CONTAINER ID ，复制这个 ID 号，比如为 e5d8c1030724

执行下面的命令提交产生 ubuntu 新版本的镜像：
``` bash?linenums
docker commit -m 'add ssh' -a 'biaowong' e5d8c1030724 ubuntu-ssh
```
参数说明：
-m，指定提交信息
-a，指定提交者
你需要把 e5d8c1030724 替换为您的容器的 CONTAINER ID
ubuntu-ssh 是新镜像的名称，可以随意指定

查看当前安装的镜像
``` bash?linenums
docker image ls
```

### 4. 创建新 Ubuntu Images
``` bash?linenums
docker run -d -p 23:22 -p 3307:3306 -p 81:80 -p  6380:6379 --name ubuntu-dev -v ~/www:/www:rw ubuntu-ssh /usr/sbin/sshd -D
```

参数说明：
| 参数              | 值         | 含义                                                         |
| ----------------- | ---------- | ------------------------------------------------------------ |
| -d                | 无         | 后台运行                                                     |
| -p                | 23:22      | 绑定主机的 23 端口到 ubuntu 容器的 22 端口（ssh服务的默认端口为 22）以此类推 |
| –name             | ubuntu-dev | 指定容器名称为 ubuntu-dev                                    |
| –v                | ~/www:/www | 将主机中当前目录下的 ~/www 挂载到容器的 /www                 |
| ubuntu-ssh        | 无         | 使用镜像 ubuntu-ssh 创建容器                                 |
| /usr/sbin/sshd -D | 无         | 指定容器启动使用的应用及参数                                 |

### 5. 配置 MacOS 登录 SSH
在 macOS 的终端中执行命令 ssh -p 2022 root@localhost 即可连接已经启动的 ubuntu 容器 learn
为了更方便的连接，可以为容器创建 ssh 连接的主机短名，往 macOS 的 ~/.ssh/config 中添加以下内容：

``` bash?linenums
Host ubuntu-dev
    HostName localhost
    User     root
    Port     2022
```
此时就可以通过命令 ssh learn 连接 ubuntu 容器 learn 了。

## 二、PHP MySQL Nginx Redis 部署

### 1. 安装 Nginx
``` bash?linenums
# 进入 Docker 系统
ssh ubuntu-dev
# 更新系统
apt update
apt upgrade
# 安装 Nginx
apt install nginx
```

### 2. 安装 MySQL
``` bash?linenums
# 安装最新版 MySQL
apt install mysql-server
# 运行安全脚本
mysql_secure_installation
```
注意：在撰写本文时，本机MySQL PHP库mysqlnd 不支持 caching_sha2_authentication ，这是MySQL 8的默认身份验证方法。因此，在MySQL 8上为PHP应用程序创建数据库用户时，您需要确保将它们配置为使用mysql_native_password代替。 
``` sql?linenums
# 进入 MySQL
mysql -uroot -p

# 创建用户并授权
CREATE USER 'db_manager'@'%' IDENTIFIED WITH mysql_native_password BY "1Qaz2Wsx";
GRANT ALL PRIVILEGES ON *.* TO 'db_manager'@'%';
flush privileges;
```

#### 允许远程访问
因为默认 mysqld.cnf 绑定 127.0.0.1 地址，所以主机无法远程访问MySQL。需修改绑定地址为 0.0.0.0 并重启后方可访问。
``` ini?linenums
vim /etc/mysql/mysql.conf.d/mysqld.cnf
bind-address = 0.0.0.0 # 127.0.0.1 修改为 0.0.0.0
# 重启 MySQL
/etc/init.d/mysql restart
```

## 3. 安装 PHP 5.6
``` ini?linenums
# 用于添加 ppa 源的小工具，ubuntu server 默认没装
apt install software-properties-common
add-apt-repository ppa:ondrej/php
apt update
apt upgrade

# 安装 PHP
apt install openssl php5.6 php5.6-fpm php5.6-common php5.6-json php5.6-mbstring php5.6-xml php5.6-zip php5.6-opcache php5.6-mcrypt php5.6-cli php5.6-gd php5.6-curl php5.6-mysql php5.6-redis php5.6-bcmath php5.6-bz2 php5.6-mongo php5.6-sqlite3 

# 修改 PHP 配置文件
vim /etc/php/5.6/fpm/pool.d/www.conf
; listen = /run/php/php5.6-fpm.sock ; 注释掉该行
listen = 127.0.0.1:9001 ; 添加该行，改为端口形式监听

# 重启 PHP
/etc/init.d/php5.6-fpm restart
```

## 4. 安装 PHP 7.3
``` ini?linenums
# 安装 PHP
apt install openssl php7.3 php7.3-fpm php7.3-common php7.3-json php7.3-mbstring php7.3-xml php7.3-zip php7.3-opcache php7.3-mcrypt php7.3-cli php7.3-gd php7.3-curl php7.3-mysql php7.3-redis php7.3-bcmath php7.3-bz2 php7.3-mongo php7.3-sqlite3 

# 修改 PHP 配置文件
vim /etc/php/7.3/fpm/pool.d/www.conf
; listen = /run/php/php7.3-fpm.sock ; 注释掉该行
listen = 127.0.0.1:9000 ; 添加该行，改为端口形式监听

# 重启 PHP
/etc/init.d/php7.3-fpm restart
```

### 配置 Nginx
修改默认配置
``` nginx?linenums
vim /etc/nginx/sites-enabled/default
#listen 80 default_server;
#listen [::]:80 default_server;
listen 80;
```

自定义配置文件
``` nginx?linenums
vim /etc/nginx/conf.d/xxx.test.conf
server {
    listen 80;
    server_name xxx.test;
    root /www/xxx-web/public;
    # 注意这个error_log，有些错误可能页面不会展示出来，但是可以通过nginx的error_log查询出来
    # error_log /www/xxx.test.error.log;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index  index.html index.htm index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    # error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass   127.0.0.1:9001;
        #fastcgi_pass   /run/php/php5.6-fpm.sock;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
         deny all;
    }
}
```

## 5. 安装 Redis
``` bash?linenums
apt install redis
```

## 错误整理
``` sql?linenums
SQLSTATE[HY000] [2054] The server requested authentication method unknown to the client
# 解决办法: 
# 1. 修改密码加密方式
ALTER USER ‘username‘@‘%‘ IDENTIFIED WITH mysql_native_password
# 或
ALTER USER ‘username‘@‘%‘ IDENTIFIED WITH mysql_native_password BY ‘new_password‘;
# 2. 修改 mysql 配置文件，添加： default-authentication-plugin=mysql_native_password
vim mysql.conf.d/mysqld.cnf
# 在最末尾添加
default-authentication-plugin=mysql_native_password
```