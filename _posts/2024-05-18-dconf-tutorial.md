---
title: dconf tutorial
date: 2024-05-18
---

# dconf 教程

禁止Gnome自动挂载

```sh
gsettings set org.gnome.desktop.media-handling automount false
gsettings set org.gnome.desktop.media-handling automount-open false 
systemctl restart gdm.service
```