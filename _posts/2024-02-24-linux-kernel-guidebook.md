---
title: linux kernel guidebook
date: 2024-02-24
---

# Linux 内核指南

理解什么是Linux内核并完成内核编译及引导

## 内核编译与引导

### Install Kernel guided in EFI stub

1. 编译前的配置

   选择被编译进内核的部分(*)，作为模块化载入内核的部分(M)，既不编译进内核也不作为模块化的部分()

   ```sh
   make menuconfig
   ```

   If you don't know how to configure it, you might as well take a look at [the kernel compilation configuration of archlinux](https://gitlab.archlinux.org/archlinux/packaging/packages/linux)

   The following command Make the old configuration file effective for the new kernel source:

   ```sh
   make oldconfig
   ```

2. 编译内核

   ```sh
   make -j12
   # -j<number> number通常为逻辑cpu的数量，用于开启多线程加速编译
   ```

   生成内核映像（通常是vmlinuz，位于内核源代码目录下的 `arch/<架构>/boot/` 目录中），同时产生 System.map 映射文件以及其他一些必要的文件。

   如果 `make menuconfig` 的更改不需要内核编译，则不会进行内核的编译，比如增加模块(M)。


3. 安装模块

   ```sh
   make modules_install
   ```

   编译并安装内核模块（kernel modules）。安装的模块文件通常被放置在系统的 `/lib/modules/<kernel_version>/` 目录中。

   内核模块是一种动态加载到内核中的二进制代码，用于支持设备驱动、文件系统、网络协议等功能。

4. Install kernal

   Install kernel manually, for gentoo:

   ```sh
   cp ./arch/x86/boot/bzImage /efi/EFI/gentoo/bzImage.efi
   ```

    The following command is **NOT RECOMMENDED**:

   ```sh
   make install
   ```

   This command will cp `vmlinuz`  and  `System.map` to  `/boot`, but it is not suitable for every host environment.

5. 生成 initramfs

   ```sh
   # `ls /lib/modules/` show all available value of `<kernel_version>`
   dracut /efi/EFI/gentoo/initrd.cpio.gz --kver <kernel_version> --force
   ```

   `initramfs` 是一个用于创建临时的文件系统，确保系统能够成功启动并进入实际的根文件系统。

   `dracut` 名字来源于两个单词的组合："drastic" 和 "cutting"。

6. 设置引导

   If `-u` or `--loader` not change, it is not necessary to execute it after updating kernel.

   ```sh
   efibootmgr --create --disk /dev/nvme1n1 --label "Gentoo" --loader "\EFI\gentoo\bzImage.efi" -u 'root=UUID=97ffc05a-6098-4af9-9d61-a40f403fffca initrd=\EFI\gentoo\initrd.cpio.gz'
   ```

More about EFI Stub can check following links:

- [gentoo efi stub](https://wiki.gentoo.org/wiki/EFI_stub#Installation)
- [gentoo reply about efi stub](https://forums.gentoo.org/viewtopic-p-8805827.html#8805827)

## 模块概念

### 内核的加载

- 系统启动时自动加载： 在系统启动时，由启动脚本（如 /etc/rc.d 或 /etc/init.d 中的脚本）或者 systemd 服务来加载一些内核模块。这些脚本通常位于系统的启动过程中，以确保在引导时加载必要的模块。
- 手动加载： 使用 modprobe 命令可以手动加载指定的内核模块。

### kmod

对于 Linux 模块的管理离不开 `kmod` 。

**Command**

- modprobe： 用于加载、卸载和管理内核模块。它会自动解析模块之间的依赖关系，并加载所需的模块。例如，modprobe <module_name> 可以加载指定的模块。
- lsmod： 显示当前加载的内核模块列表。可以使用 lsmod 命令查看已加载的模块及其使用情况。
- rmmod： 用于从内核中卸载已加载的模块。例如，rmmod <module_name> 可以卸载指定的模块。
- insmod： 用于将指定的模块加载到内核中。与 modprobe 不同，insmod 不会解析模块依赖关系，因此需要手动处理任何依赖关系。
- depmod： 生成模块依赖关系文件。这个工具会扫描已安装的模块，并生成一个 modules.dep 文件，其中包含模块之间的依赖关系。
- modinfo： 显示有关指定模块的信息，如模块的作者、版本、描述等。例如，modinfo <module_name> 可以获取有关指定模块的详细信息。
