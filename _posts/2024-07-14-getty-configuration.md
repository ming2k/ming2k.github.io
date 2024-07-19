---
title: getty configuration
date: 2024-07-14
---

# Getty Configuration

edit `/etc/systemd/system/getty@tty1.service.d/autologin.conf`

```sh
sudo nvim /etc/systemd/system/getty@tty1.service.d/autologin.conf
```

add the following content:

```service
[Service]
ExecStart=
ExecStart=-/sbin/agetty -n -o <username> %I
```

system reload

```sh
systemctl daemon-reload
```

restart tty1

```sh
sudo systemctl restart getty@tty1.service
```
