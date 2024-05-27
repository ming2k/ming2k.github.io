---
title: common command usage for linux operator
date: 2024-05-27
---

# Linux操作员的常见命令用法

旨在简略介绍常用命令的使用方法。



## 进程查询

The `ss` command is `iproute2` package and stands for "Socket Statistics".

```sh
ss -tnlp
# options description:
# t: tcp;
# u: udp;
# n: numberic, it means not trying to resolve the service names;
# l: display only listening sockets;
# p: show process using socket;
# e: extend
```

Output for `ss -tunlpe`:

```
LISTEN   0         128                127.0.0.1:6000              0.0.0.0:*      users:(("process_name",pid=1234,fd=3)) uid:1000 ino:29160 sk:101f cgroup:/user.slice/user-1000.slice/user@1000.service/app.slice/mpd.service v6only:0 <->
```

The following table will be helpful on learning what the every column means:

| status | Recv-Q | Send-Q | Local Adress   | Peer Adress | User/Processes                         | Detailed Info                                         |
| ------ | ------ | ------ | -------------- | ----------- | -------------------------------------- | ----------------------------------------------------- |
| LISTEN | 0      | 128    | 127.0.0.1:6000 | 0.0.0.0:*   | users:(("process_name",pid=1234,fd=3)) | uid:`<uid_number>` ino:`<inode_number> `sk:`<cookie>` |

## 根据UID查询

```sh
awk -F: '$3 == 1000 {print $1}' /etc/passwd
```

