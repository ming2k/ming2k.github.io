---
title: gentoo linux from scratch
date: 2024-02-23
---

# Gentoo 安装指南

在拥有 Arch Linux 安装经验后，其中很多部分比较容易理解。

[官方文档](https://wiki.gentoo.org/wiki/Handbook:AMD64)有很详尽的操作，这个教程作为概述以及补充。

## 烧录镜像

安装 Gentoo 的 Live 镜像要求有 Linux 的基本工具。这里可以选择 Ubuntu 进行安装 Gentoo 。

## 内核编译及引导

### 手动编译 + EFI Stub

编译安装：

```sh
# config
make menuconfig
make -j12 && make modules_install
make install
```

引导安装：

```sh
cp ./arch/x86/boot/bzImage /efi/EFI/gentoo/bzImage.efi
# initramfs
dracut /efi/EFI/gentoo/initrd.cpio.gz --kver <kernel_version>
# <kernel version> 为 `/lib/modules/` 下的目录名
# for example 
dracut /efi/EFI/gentoo/initrd.cpio.gz --kver  6.6.30-gentoo-x86_64
# 注册 efi （仅需第一次）
# 这里要根据情况对具体参数进行修改
efibootmgr --create --disk /dev/nvme1n1 --label "Gentoo" --loader "\EFI\gentoo\bzImage.efi" -u 'root=UUID=97ffc05a-6098-4af9-9d61-a40f403fffca initrd=\EFI\gentoo\initrd.cpio.gz'
```

### install-kernel 安装（OpenRC） + GRUB

详情查看 [Installkernel - Gentoo](https://wiki.gentoo.org/wiki/Installkernel) 

### 内核更新维护

#### 更新内核及引导

默认情况下，Gentoo 的内核源码存放在 `/usr/src` ，该目录下有一个 symlink 指向默认的 linux 内核版本，你可以通过删除并创建新的符号连接更新它，也可以通过下述命令行自动更新它：

```sh
eselect kernel list
eselect kernel set <number>
```

##### 手动编译 + EFI Stub

1. 更新 `.config` 。将之前 `/usr/src/linux-<old_version>` 目录下通过 `make menuconfig` 产生的 `.config` 拷贝的 `/usr/src/linux-<new_version>` 的目录下，并执行下述命令更新 `.config`：

    ```sh
    make oldconfig
    ```

2. 编译安装：

    ```sh
    make -j12 && make modules_install
    ```

3. 跟新引导关联的文件：

    ```sh
    cp ./arch/x86/boot/bzImage /efi/EFI/gentoo/bzImage.efi
    dracut /efi/EFI/gentoo/initrd.cpio.gz --kver <kernel_version>
    ```

4. 重启系统

## Trouble

**MT7921蓝牙不工作**

内核配置中配置：

```
CONFIG_BT_MTK=m
CONFIG_BT_HCIBTUSB_MTK=y
```

仍有问题，后续持续修改内核配置，添加对各种蓝牙驱动协议支持（一通乱加），Sony的蓝牙耳机的LDAC可以用了。

---

**触摸板不工作**

参考：https://wiki.gentoo.org/wiki/Synaptics

---

 **obs无法录屏**

obs没有开启 pipewire use 标记

---

**ibus输入法连续触发的问题**

安装 `app-i18n/im-chooser`

---

**VSCode 无法输入中文**

删除 vscode desktop 文件`Exec`后面的参数：

```sh
sudo nvim /usr/share/applications/code.desktop
```

保留最后像这样：

```desktop
Exec=/usr/bin/vscode %F
```
