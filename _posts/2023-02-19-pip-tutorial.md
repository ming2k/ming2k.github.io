---
title: pip tutorial
date: 2023-02-19
---

# pip tutorial

pip 教程

## 基本操作

### 安装包

用户级别安装

```sh
pip install <PACKAGE>
# is equivalent to
pip install --user <PACKAGE>
# 在非root权限下默认安装级别为用户级
```

系统级别安装

```sh
sudo pip install <PACKAGE>
```

安装升级包

```sh
pip install --upgrade <OUTDATED_PACKAGE>
```

### 列出包

列出所有包

```sh
pip list
```

列出用户级别的包

```sh
pip list --user
```

列出过时包

```sh
pip list --outdated
```

### 查看包信息

```sh
pip show <PACKAGE>
```

*如果 `pip` 或 `pip3` 命令无法找到，可以尝试使用 `python3 -m <command>` 。*

## 进阶操作

更新全部：

```sh
pip list --outdated | cut -d ' ' -f 1 | xargs -n1 pip install --upgrade
```

清理缓存

```sh
pip cache purge
```



## 安装路径

## Linux

全局：``/usr/lib/python3.11/site-packages`

用户：`/home/<USERNAME>/.local/lib/python3.11/site-packages`

### Mac

全局：`/usr/local/opt/python@3.9/Frameworks/Python.framework/Versions/3.9/bin`

用户：`/Users/<USERNAME>/Library/Python/3.8/bin`

## Q&A

Q: `This environment is externally managed`

You can use python `venv` like described [here](https://stackoverflow.com/a/75696359/10626495).

---

However if you really want to install packages that way, then there are couple solutions:

- remove file `/usr/lib/python3.x/EXTERNALLY-MANAGED`,
- use `pip`'s argument `--break-system-packages`,
- add following lines to `~/.config/pip/pip.conf`:

```py
[global]
break-system-packages = true
```

https://stackoverflow.com/questions/75608323/how-do-i-solve-error-externally-managed-environment-everytime-i-use-pip3

---

WARNING: Ignoring invalid distribution -ip

原因可能是之前下载库的时候没有成功或者中途退出。
解决方法：
到提示的目录site-packages下删除~ip开头的目录。
然后pip重新安装库即可。