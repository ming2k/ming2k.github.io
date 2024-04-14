---
title: make linux prefer IPv4
date: 2024-04-12
---

# 设置 Linux 偏好 IPv4

如今现在很多情境下 `IPv6` 仍然不是很好用。比如默认情况下 Gentoo 解析 gitlab 默认使用 IPv6，gitlab 的 IPv6 进行 ssh 连接 push 代码会失败，需要使用 gitlab 的 `IPv4`.

关于如何设置偏好，更多可以查看[Make Linux Prefer IPv4](https://weblog.lkiesow.de/20220311-make-linux-prefer-ipv4.html)博客内容。

```
sudo nvim /etc/gai.conf
```

写入以下配置：

```
label  ::1/128       0
label  ::/0          1
label  2002::/16     2
label ::/96          3
label ::ffff:0:0/96  4
precedence  ::1/128       50
precedence  ::/0          40
precedence  2002::/16     30
precedence ::/96          20
precedence ::ffff:0:0/96  100
```

