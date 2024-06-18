---
title: gentoo package management tutorial
date: 2024-02-23
---

## Gentoo Package Management Tutorial

该教程致力于Gentoo包管理教学使用。

## 查找

### emerge

查看组内安装了哪些包：

```sh
emerge -pqeO <set_name>

# for example: 
# emerge -pqeO @system # list system required pakcages
# emerge -pqeO @world # list world-wide pakcages 
```

### equery

```sh
emerge -s <package_name>
emerge --search <package_name>
emerge --searchdesc <package_description>

# for example: 
# emerge -s '%^python$' # search using a regex
# emerge -s '@net-ftp' # search catagory
```

### eix

```sh
eix <package_name>
```



## Portage CMD

### Emerge

同步安装包信息：

```sh
emerge --sync
```

安装

```sh
emerge <pkg_name>
# with ask
emerge -a <pkg_name>
emerge --ask <pkg_name>
```

卸载：

```sh
emerge -C <pkg_name>
emerge --unmerge <pkg_name>
```

清理已安装软件包的构建残留物：

```sh
emerge -c
emerge --depclean
```

清理配置文件

```sh
sudo emerge --config sys-apps/portage  
```

全局更新：

```sh
# GNU style
emerge --ask --verbose --update --deep --newuse @world
# --deep: forces  emerge to consider the entire dependency tree of packages, instead of checking only the immediate dependencies of the packages.
# --newuse: tells emerge to include installed packages where USE flags have changed since compilation.

# or Unix/POSIX style
emerge -avuDN @world
```

清理依赖：

```sh
# GNU style
sudo emerge --depclean       # 删除不再需要的软件包及其依赖项
# or Unix/POSIX style
sudo emerge -c
```

### Eclean

```sh
sudo eclean distfiles	# Clean files from /var/cache/distfiles
# or --deep to clean more
sudo eclean -d distfiles

sudo eclean packages	# Clean files from /usr/portage/packages
# or --deep to clean more
sudo eclean -d packages
```

## Equery

查看安装的包：

```sh
equery list '*'
```

查看包的 `use` 选项和介绍：

```sh
equery uses <pkg_name>
```

查看包安装文件路径：

```sh
equery files <package_name>
```

查看文件归属的包：

```sh
equery belongs <file_path>
# for example
equery belongs /usr/share/applications/wpa_gui.desktop
```

### Eselect

待补充

## 配置

### 编译配置

`/etc/portage/make.conf` 文件负责下载、编译、安装相关配置。

### USE

路径：`/etc/portage/package.use` or `/etc/portage/package.use/*`

用于设置包的use选项，文件格式为：

```
<pkg> <use_list>
```

### KEYWORDS

待补充：KEYWORDS设计的目的是为了解决什么问题

路径：`/etc/portage/package.accept_keywords` or `/etc/portage/package.accept_keywords/*`

内容格式为：

```
<pkg> <keywords>
```

## Repo

### 添加仓库

```sh
# 安装第三方仓库管理插件
emerge --ask app-eselect/eselect-repository
# 安装git（大部分第三方仓库都是git管理）
emerge --ask dev-vcs/git
# 查看[第三方仓库列表](Gentoo Overlays)
eselect repository list
# 启用第三方仓库（开这两个就基本够用了），guru 是 gentoo 官方的用户维护的，而 gentoo-zh 更适合中国人体质
eselect repository enable guru gentoo-zh
# 更新所有仓库，可以加 -r <仓库名> 更新指定仓库
emerge --sync
```

### 删除仓库

#### 通过 `eselect` 工具删除

```sh
# 移除指定的 overlay
sudo eselect repository remove my-overlay
```

#### 手动删除

```sh
sudo rm /etc/portage/repos.conf/my-overlay.conf
sudo rm -rf /var/db/repos/my-overlay
```

### 创建自己的仓库

1. 创建一个 git 仓库，上传到 `github` 或 `gitlab` 任一 git 代码仓库托管平台；

2. 在仓库中按照如下内容编写

   ```
   ├── app-misc
   │   └── goldendict
   │       ├── goldendict-9999.ebuild
   │       └── metadata.xml
   ├── metadata
   │   └── layout.conf
   └── profiles
       └── repo_name
   ```

   layout.conf 内容为：

   ```conf
   masters = gentoo
   thin-manifests = true
   ```

   repo_name 内容为：

   ```
   pure
   ```

   app-misc 目录下为包相关内容

3. 编写本地机器的portage的repo配置文件

   以我编写的名为 `pure` 仓库作为 overlay 的示例，编辑 `/etc/portage/repos.conf/pure.conf` 文件：

   ```sh
   [pure]
   location = /var/db/repos/pure
   sync-type = git
   sync-uri = https://github.com/fox20431/pure-overlay.git
   ```

   这里注意 `[pure]` 要与仓库的 `repo_name` 保持一致。

   