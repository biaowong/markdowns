# Ubuntu 20.04 SSH 登录添加 Google Authenticator 两步验证

#### 安装 Google Authenticator

```shell
sudo apt install libpam-google-authenticator -y
```

#### 修改相关 sshd 文件配置

```shell
sudo vim /etc/pam.d/sshd # 编辑 sshd 文件，在末尾添加如下内容
# Google Authenticator
auth required pam_google_authenticator.so

sudo vim /etc/ssh/sshd_config
ChallengeResponseAuthentication yes # 默认是 no 改为 yes

# 重启 ssh 服务
sudo systemctl restart sshd
```

#### 生成二维码和秘钥，全选 y

```shell
google-authenticator
```

通过 Google Authenticator App 扫描生成的二维码或输入生成的秘钥来添加验证权限。