---
title: back up for linux
date: 2023-12-23
---

# Backup for Linux

该教程为 Linux 平台下类 Mac 平台下的 `Time Machine` 的解决方案。

*Mac下的 Time Machine 的特性是可以记录不同时间点的文件状态，且是增量更新。*

## 可行性方案

通过查询发现可行性解决方案有以下几种：

- [timeshift](https://github.com/linuxmint/timeshift)
- [Btrfs Assistant](https://gitlab.com/btrfs-assistant/btrfs-assistant)
- [btrbk](https://github.com/digint/btrbk)
- DIY in `btrfs`

最后选择了 `DIY in btrfs` 的方式进行备份。

To save my time and energy, I have write a script to help me backup quickly. You can access [the github repo](https://github.com/ming2k/time-machine-for-linux-script) if you are interested in it.

## 标准操作

该操作流程适合初次使用该教程的新人。

1. First, you need to create a Btrfs partition on your backup device. You can use a tool like parted or fdisk to create a new partition and format it as Btrfs.

    以下为常用命令

    ```sh
    # 格式分区为btrfs文件系统
    sudo mkfs.btrfs /dev/partition
    # 更改btrfs文件的label
    sudo btrfs filesystem label /path/to/mounted/btrfs_volume new_label
    ```

2. Once you have the Btrfs partition, you can use the btrfs subvolume create command to create a new subvolume on the partition, which will be used to store the backup.

    ```sh
    sudo btrfs subvolume create /path/to/backup/partition/backup
    ```

3. To backup the ext4 file system, you can use the rsync command to copy the entire file system to the new Btrfs subvolume.

    ```sh
    # a archive 
    # v verbose
    # x cross file bo
    sudo rsync -av	xHAX --numeric-ids --delete --checksum / /path/to/backup/partition/backup
    ```

4. Once the backup is complete, you can take a snapshot of the subvolume with the btrfs subvolume snapshot command.

    ```sh
    sudo btrfs subvolume snapshot -r /path/to/backup/partition/backup /path/to/backup/partition/snapshot
    ```

    delete the snapshot:

    ```sh
    sudo btrfs subvolume delete <snapshot>
    ```

5. 恢复系统。进入Live镜像（要求该镜像支持btrfs以及用用rsync工具），挂载要恢复的分区，逆向的备份方法在拷贝回去，详情间下述命令：

    ```sh
    sudo rsync -avxHAX --numeric-ids --delete --checksum /path/to/backup/partition/backup  /path/to/backup/partition/restore
    ```

    使用 `arch-chroot <mount point>` 进入到构建的系统，重新生成`fstab`以及重建grub引导。

## 进阶操作

合并并优化上述教程的第三和第四步，使用我个人编写的脚本：[time-machine-for-linux-script](https://github.com/fox20431/time-machine-for-linux-script)。

## 存储介质

### M.2 SSD + 移动硬盘盒

以下为个人方案

硬盘盒：嘉翼（JEYI） NVME硬盘盒 M.2转Type-C移动硬盘盒 USB3.1固态SSD全铝硬盘盒 10Gbps 太空灰 | i9 GTR-2280

该硬盘盒采用的主控芯片为：RTL9210B

硬盘：CRUCIAL/镁光 P5固态硬盘 2TB M.2 2280 SSD

Pros: High Speed; Cheap; DIY

Cons: Overheating during work; Unstable under low voltage

**市场上移动硬盘盒协议兼容问题、温控问题，不推荐**

### 一体式硬盘盒

三星T7
