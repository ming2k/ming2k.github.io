---
title: pipx tutorial
date: 2024-05-09
---

# pipx 教程

pipx是一个用于管理Python包的工具，它允许你在全局环境中安装Python包，同时避免了包之间的冲突。这篇教程将向你展示如何使用pipx来安装和管理Python包。

## 安装 pipx

```sh
pip install --user pipx
```

## 使用 pipx 安装包

```sh
pipx install example-package
```

## 管理安装的包

- `pipx list`: 显示当前安装的所有包。
- `pipx uninstall package_name`: 卸载指定的包。
- `pipx upgrade package_name`: 升级指定的包。

## 使用安装的包

通过 `pipx` 安装的包可以在全局环境中直接使用。

