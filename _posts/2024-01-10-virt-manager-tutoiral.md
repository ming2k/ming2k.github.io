---
title: virt manager tutorial
date: 2024-01-10
---



# Virt Manager 教程

`libvirt` 提供了软件集合用于方便管理多种虚拟机和虚拟化。

> A primary goal of libvirt is to provide a single way to manage multiple different virtualization providers/hypervisors, such as the [KVM/QEMU](https://wiki.archlinux.org/title/QEMU), [Xen](https://wiki.archlinux.org/title/Xen), [LXC](https://wiki.archlinux.org/title/LXC), [OpenVZ](https://openvz.org/) or [VirtualBox](https://wiki.archlinux.org/title/VirtualBox) [hypervisors](https://wiki.archlinux.org/title/Category:Hypervisors) ([among others](https://libvirt.org/drivers.html)).

`virt-manager` 是 `libvirt` 的前端图形化实现方案。

## Install and basic Settings

安装 `libvirt` 和 `virt-manager` ：

```sh
# edk2-ovmf is to support UEFI
# swtpm is to support for TPM
sudo pacman -S qemu-full virt-manager edk2-ovmf swtpm
```

The KVM/QEMU driver is primary libvirt driver.

启用服务：

```sh
sudo systemctl enable --now libvirtd
```

将用户加入到 `libvirt` 组中以获得 `libvirt`  的操作权限：

```sh
usermod -a -G libvirt $your_username
```

## Create a VM by libvirt

1. 在 xml 中定义虚拟机资源：

    ```xml
    <!-- 仅供参考！！！ -->
    <domain type='hvf' xmlns:qemu='http://libvirt.org/schemas/domain/qemu/1.0'>
        <name>ubuntu</name>
        <uuid>2005CB24-522A-4485-9B9A-E60A61D9F8CF</uuid>
        <memory unit='GB'>4</memory>
        <cpu mode='custom'>
            <model>Westmere</model>
        </cpu>
        <vcpu>4</vcpu>
        <features>
            <acpi />
            <apic />
        </features>
        <os>
            <type arch='x86_64' machine='q35'>hvm</type>
            <bootmenu enable='yes' />
        </os>
        <clock offset='localtime' />
        <on_poweroff>destroy</on_poweroff>
        <on_reboot>restart</on_reboot>
        <on_crash>destroy</on_crash>
        <pm>
            <suspend-to-mem enabled='no' />
            <suspend-to-disk enabled='no' />
        </pm>
        <devices>
            <emulator>/usr/local/bin/qemu-system-x86_64</emulator>
            <controller type='usb' model='ehci' />
            <disk type='file' device='disk'>
                <driver name='qemu' type='qcow2' />
                <source file='/Users/ming/vms/ubuntu.qcow2' />
                <target dev='vda' bus='virtio' />
            </disk>
            <disk type='file' device='cdrom'>
                <source file='/Users/ming/vms/ubuntu.iso' />
                <target dev='sdb' bus='sata' />
            </disk>
            <console type='pty'>
                <target type='serial' />
            </console>
            <input type='tablet' bus='usb' />
            <input type='keyboard' bus='usb' />
            <graphics type='vnc' port='5900' listen='127.0.0.1' />
            <video>
                <model type='virtio' vram='16384' />
            </video>
        </devices>
        <seclabel type='none' />
        <qemu:commandline>
            <qemu:arg value='-machine' />
            <qemu:arg value='type=q35' />
        </qemu:commandline>
    </domain>
	```


2. 用 `virtsh` 工具启动虚拟机：

    ```sh
    # 使定义文件生效
    virtsh define $XML_FILE
    # 根据定义文件的虚拟机名称，启动虚拟机
    virsh start ubuntu
	```

3. 控制虚拟机行为：

    ```sh
    # 关闭虚拟机
    virsh shutdown ubuntu
    # 销毁虚拟机
    virsh destroy ubuntu
    # 保存虚拟机，下次可以用start继续运行
    virsh managedsave ubuntu
    ```

> 提示：` libvirt` 的 image 默认存储到`/var/lib/libvirt/images`

## 使用 Virt Manger 创建虚拟机

virt-manger 具有可视化图形界面，更方便使用。

```sh
systemctl enable virtnetworkd
```

启动网卡：

```sh
sudo virsh net-start default
# 设置为自动启动
sudo virsh net-autostart default
```

**Where is the installation files related vm?**

 qcow2 location: /var/lib/libvirt/images

xml location: /etc/libvirt/qemu/

