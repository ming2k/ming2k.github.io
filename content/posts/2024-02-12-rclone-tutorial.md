---
title: rclone tutorial
date: 2024-02-12
---

# RClone 教程

环境：Linux

平台：OneDrive

参考文献：

- [Linux使用RClone挂载OneDrive网盘](https://333rd.net/posts/tech/linux%E4%BD%BF%E7%94%A8rclone%E6%8C%82%E8%BD%BDonedrive%E7%BD%91%E7%9B%98/)

使用该云盘方案从文件操作感受上类似挂载了一个物理硬盘，可以选择性挂载位置，也可以解挂载。

1. 安装 rclone

   gentoo 安装：

   ```sh
   emerge -a net-misc/rclone
   emerge -a sys-fs/fuse
   ```

2. 配置 `rclone`

   进入配置的交互界面命令：

   ```sh
   rclone config
   ```

   交互界面具体操作示例（以OneDrive为例）：

   ```
   ming@assassin:~$ rclone config
   No remotes found, make a new one?
   n) New remote
   s) Set configuration password
   q) Quit config
   n/s/q> n
   
   Enter name for new remote.
   name> Main OneDrive
   
   Option Storage.
   Type of storage to configure.
   Choose a number from below, or type in your own value.
    1 / 1Fichier
      \ (fichier)
    2 / Akamai NetStorage
      \ (netstorage)
    3 / Alias for an existing remote
      \ (alias)
    4 / Amazon Drive
      \ (amazon cloud drive)
    5 / Amazon S3 Compliant Storage Providers including AWS, Alibaba, Ceph, China Mobile, Cloudflare, ArvanCloud, DigitalOcean, Dreamhost, Huawei OBS, IBM COS, IDrive e2, IONOS Cloud, Liara, Lyve Cloud, Minio, Netease, RackCorp, Scaleway, SeaweedFS, StackPath, Storj, Tencent COS, Qiniu and Wasabi
      \ (s3)
    6 / Backblaze B2
      \ (b2)
    7 / Better checksums for other remotes
      \ (hasher)
    8 / Box
      \ (box)
    9 / Cache a remote
      \ (cache)
   10 / Citrix Sharefile
      \ (sharefile)
   11 / Combine several remotes into one
      \ (combine)
   12 / Compress a remote
      \ (compress)
   13 / Dropbox
      \ (dropbox)
   14 / Encrypt/Decrypt a remote
      \ (crypt)
   15 / Enterprise File Fabric
      \ (filefabric)
   16 / FTP
      \ (ftp)
   17 / Google Cloud Storage (this is not Google Drive)
      \ (google cloud storage)
   18 / Google Drive
      \ (drive)
   19 / Google Photos
      \ (google photos)
   20 / HTTP
      \ (http)
   21 / Hadoop distributed file system
      \ (hdfs)
   22 / HiDrive
      \ (hidrive)
   23 / In memory object storage system.
      \ (memory)
   24 / Internet Archive
      \ (internetarchive)
   25 / Jottacloud
      \ (jottacloud)
   26 / Koofr, Digi Storage and other Koofr-compatible storage providers
      \ (koofr)
   27 / Local Disk
      \ (local)
   28 / Mail.ru Cloud
      \ (mailru)
   29 / Mega
      \ (mega)
   30 / Microsoft Azure Blob Storage
      \ (azureblob)
   31 / Microsoft OneDrive
      \ (onedrive)
   32 / OpenDrive
      \ (opendrive)
   33 / OpenStack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
      \ (swift)
   34 / Oracle Cloud Infrastructure Object Storage
      \ (oracleobjectstorage)
   35 / Pcloud
      \ (pcloud)
   36 / Put.io
      \ (putio)
   37 / QingCloud Object Storage
      \ (qingstor)
   38 / SMB / CIFS
      \ (smb)
   39 / SSH/SFTP
      \ (sftp)
   40 / Sia Decentralized Cloud
      \ (sia)
   41 / Storj Decentralized Cloud Storage
      \ (storj)
   42 / Sugarsync
      \ (sugarsync)
   43 / Transparently chunk/split large files
      \ (chunker)
   44 / Union merges the contents of several upstream fs
      \ (union)
   45 / Uptobox
      \ (uptobox)
   46 / WebDAV
      \ (webdav)
   47 / Yandex Disk
      \ (yandex)
   48 / Zoho
      \ (zoho)
   49 / premiumize.me
      \ (premiumizeme)
   50 / seafile
      \ (seafile)
   Storage> 31
   
   Option client_id.
   OAuth Client Id.
   Leave blank normally.
   Enter a value. Press Enter to leave empty.
   client_id> <YOUR_CLIENT_ID>
   
   Option client_secret.
   OAuth Client Secret.
   Leave blank normally.
   Enter a value. Press Enter to leave empty.
   client_secret> <YOUR_CLIENT_SECRET>
   
   Option region.
   Choose national cloud region for OneDrive.
   Choose a number from below, or type in your own string value.
   Press Enter for the default (global).
    1 / Microsoft Cloud Global
      \ (global)
    2 / Microsoft Cloud for US Government
      \ (us)
    3 / Microsoft Cloud Germany
      \ (de)
    4 / Azure and Office 365 operated by Vnet Group in China
      \ (cn)
   region> 1
   
   Edit advanced config?
   y) Yes
   n) No (default)
   y/n> n
   
   Use web browser to automatically authenticate rclone with remote?
    * Say Y if the machine running rclone has a web browser you can use
    * Say N if running rclone on a (remote) machine without web browser access
   If not sure try Y. If Y failed, try N.
   
   y) Yes (default)
   n) No
   y/n> y
   
   2024/04/18 16:27:00 NOTICE: Make sure your Redirect URL is set to "http://localhost:53682/" in your custom config.
   2024/04/18 16:27:00 NOTICE: If your browser doesn't open automatically go to the following link: http://127.0.0.1:53682/auth?state=Q6hZcLG1LjFeFqi56mJTcw
   2024/04/18 16:27:00 NOTICE: Log in and authorize rclone for access
   2024/04/18 16:27:00 NOTICE: Waiting for code...
   2024/04/18 16:27:07 NOTICE: Got code
   Option config_type.
   Type of connection
   Choose a number from below, or type in an existing string value.
   Press Enter for the default (onedrive).
    1 / OneDrive Personal or Business
      \ (onedrive)
    2 / Root Sharepoint site
      \ (sharepoint)
      / Sharepoint site name or URL
    3 | E.g. mysite or https://contoso.sharepoint.com/sites/mysite
      \ (url)
    4 / Search for a Sharepoint site
      \ (search)
    5 / Type in driveID (advanced)
      \ (driveid)
    6 / Type in SiteID (advanced)
      \ (siteid)
      / Sharepoint server-relative path (advanced)
    7 | E.g. /teams/hr
      \ (path)
   config_type> 1
   
   Option config_driveid.
   Select drive you want to use
   Choose a number from below, or type in your own string value.
   Press Enter for the default (ab8dcc8bc127f204).
    1 /  (personal)
      \ (ab8dcc8bc127f204)
   config_driveid> 1
   
   Drive OK?
   
   Found drive "root" of type "personal"
   URL: https://onedrive.live.com/?cid=ab8dcc8bc127f204
   
   y) Yes (default)
   n) No
   y/n> y
   
   Configuration complete.
   Options:
   - type: onedrive
   - client_id: <CLIENT_ID_VALUE>
   - client_secret: <CLIENT_SECRET_VALUE>
   - token: <TOKEN_VALUE>
   - drive_id: <DRIVE_VALUE>
   - drive_type: personal
   Keep this "Main OneDrive" remote?
   y) Yes this is OK (default)
   e) Edit this remote
   d) Delete this remote
   y/e/d> y
   
   Current remotes:
   
   Name                 Type
   ====                 ====
   Main OneDrive        onedrive
   
   e) Edit existing remote
   n) New remote
   d) Delete remote
   r) Rename remote
   c) Copy remote
   s) Set configuration password
   q) Quit config
   e/n/d/r/c/s/q> q
   ```

   上述配置OneDrive过程中需要 `Clent ID` 和 `Client Secret`，你需要注册在[Microsoft Azure 应用注册](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps)注册应用，注册应用后则会产生对应的 `Client ID`. then create the secret.

3. 配置完成后，可以使用下述命令测试是否具有访问权限

   ```sh
   rclone ls <REMOTE_NAME>
   # 比如，冒号不能省去
   rclone ls "Main OneDrive":
   ```

4. 挂载操作

   ```sh
   rclone mount OneDrive:Documents/KeePass ~/Documents/KeePass/  --daemon 
   ```

其他帮助参数 `-vv` 可以查看更详细的 debug 信息，更外该命令提供了很好的命令行补全功能。


### 开机自启动挂载

以其中一个挂载为例

编辑 `.config/systemd/user/rclone-onedrive.service` ：

```
[Unit]
Description=Rclone OneDrive Mount Service
# Make sure keepass exist
AssertPathIsDirectory=%h/Documents/KeePass
# Make sure we have network enabled
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/rclone mount OneDrive:Documents/KeePass %h/Documents/KeePass
ExecStop=/usr/bin/umount %h/Documents/KeePass

# Restart the service whenever rclone exists with non-zero exit code
Restart=on-failure
RestartSec=15

[Install]
# Autostart after reboot
WantedBy=default.target
```

然后执行：

```sh
# 更新配置后自动更新
systemctl --user daemon-reload
systemctl --user enable rclone-onedrive.service
```

