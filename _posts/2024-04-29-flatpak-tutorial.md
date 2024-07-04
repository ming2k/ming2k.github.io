---
title: flatpak tutorial
date: 2024-04-29
---

# flatpak tutorial

应用安装的位置：`/var/lib/flatpak`

更新：

```sh
flatpak update
```

卸载：

```sh
flatpak uninstall --unused
```

列举：

```sh
# 列出相应一列
flatpak list --columns=ref
# 只列出app
flatpak list --app
```
