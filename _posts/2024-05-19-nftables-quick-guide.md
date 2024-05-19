---
title: nftables quick guide
date: 2024-05-19
---

# nftables 快速指南

 该教程旨在快速了解并上手使用 `nftables` 。

## What is nftables

`nftables` 是 Linux 上的防火墙。Linux 存在名为 [netfilter](https://www.netfilter.org/) 内核模块，该模块允许操作数据包（packet），为了便于操作人员使用该功能先后出现了 `iptables` 以及 `nftables` ， `nftables` 是 `iptables` 的继任者（successor）。

## 没想到名字

### 路由钩子（Routing hook）

