---
title: music metadata manipulation
date: 2024-03-31
published: false
---

# 音乐文件元数据控制

最近发现 gnome music 解析音乐文件歌名和歌手乱码，是音乐文件的元数据乱码导致的。

## FFMPEG

命令行可以通过`ffmpeg`解决。

查看音乐文件的元信息：

```sh
ffmpeg -i <MUSIC_NAME>
```

修改 `medadate` ，如果设置为空则表示删除该 metadata：

```sh
ffmpeg -i "遗忘-邓丽君.mp3" -metadata title="遗忘" -metadata artist="邓丽君" -metadata album="难忘的一天" "遗忘-邓丽君1.mp3"
```

## 其他方案

kid3是KDE的修改音乐 metadata 的软件，推荐：

```sh
flatpak install kid3
```

图形化界面会更容易解决这个问题，可以安装`MusicBrainz Picard`：

```sh
flatpak install picard
```

