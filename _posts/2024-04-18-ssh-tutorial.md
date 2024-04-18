---
title: ssh tutorial
date: 2024-04-18
---

# SSH 教程

## 创建密钥

使用 RSA 创建密钥：

```sh
ssh-keygen -t rsa
```

## 服务端配置

创建 `authorized_keys` 并更新为安全的权限：

```sh
cd ~/.ssh && cat id_rsa.pub >> authorized_keys
chmod 600 authorized_keys
```

`/etc/sshd_config` 配置：

```sh
# 允许共钥
PubkeyAuthentication yes
# 禁止密码
PasswordAuthentication no
# 向客户端确认Alive间隔时间
ClientAliveInterval 60
# 向客户端确认Alive无响应最大次数
ClientAliveCountMax 3
# 允许ROOT登录
PermitRootLogin yes
```

重启服务：

```sh
systemctl restart sshd.service
```

客户端登陆：

```sh
ssh -i key.pem root@192.168.1.1
```

## 客户端配置用于快速链接

修改配置文件，设置要SSH服务器：

```sh
vim .ssh/config
```

下面是模板：

```config
host k8s-master
  hostname 192.168.122.200
  user root
  port 22
  identityfile ~/.ssh/id_rsa
```

通过配置连接服务器：

```sh
ssh k8s-master
```

