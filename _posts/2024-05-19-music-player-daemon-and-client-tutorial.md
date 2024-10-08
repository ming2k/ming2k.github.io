---
title: music player daemon and clients tutorial
date: 2024-05-19
---

# MPC&Clients 教程

[MPD](https://www.musicpd.org/)（Music Player Daemon）是音乐播放器服务端应用，顾名思义我们还需要安装客户端，比如 `mpc` 、 `ncmpc` 。

## MPD

### 安装

1. 安装MPD

   ```sh
   sudo emerge -a mpd
   ```

2. 用户级别启动服务

   ```
   systemctl enable --user --now mpd
   ```

   *除了用户级别启动，还可以系统级别启动。由于权限高于需求的权限，不推荐。*

3. 安装MPD客户端，推荐 `mpc` 和 `ncmpc` （两者都是MPD组织的子项目）

   ```sh
   sudo emerge -a mpc ncmpc
   ```

### 配置

系统级别的服务的配置文件路径为 `/etc/mpd.conf` ，用户级别的服务路径为 `~/.config/mpd/mpd.conf` 。

1. 配置用户级别的配置文件 `~/.config/mpd/mpd.conf` （若目录不存在请自行创建）

    以下为参考内容，追求个性化请查询官方文档

    ```conf
    # Recommended location for database
    db_file            "~/.config/mpd/database"
    
    # If running mpd using systemd, delete this line to log directly to systemd.
    log_file           "syslog"
    
    # The music directory is by default the XDG directory, uncomment to amend and choose a different directory
    music_directory    "~/Music"
    
    # Uncomment to refresh the database whenever files in the music_directory are changed
    #auto_update "yes"
    
    # Uncomment to enable the functionalities
    #playlist_directory "~/.config/mpd/playlists"
    #pid_file           "~/.config/mpd/pid"
    #state_file         "~/.local/state/mpd/state"
    #sticker_file       "~/.config/mpd/sticker.sql"
    
    # if your audio server is pipewire
    audio_output {
            type            "pipewire"
            name            "PipeWire Sound Server"
    }
    ```

    配置参考 [MPD - Arch Wiki](https://wiki.archlinux.org/title/Music_Player_Daemon) 。

2. 按照开启的功能，分别创建对应的文件夹：

    ```sh
    # for playlist_directory
    mkdir ~/.config/mpd/playlists
    # for state_file
    mkdir -p ~/.local/state/mpd
    ```

3. 重启服务

    ```sh
    systemctl restart --user --now mpd
    ```

## Clients

The client of `mpd` includes `mpc`, `ncmpc`, `ncmpcpp`, etc. 

Both `mpc` and `ncmpc` are official clients, with the main difference being that `ncmpc` is based on `ncurses`. Additionally, `ncmpcpp`, developed in C++, is a community-promoted client.

### mpc

mpc is a fundamental tool that includes all basic functions, making it essential to learn how to use it.

#### playlist commands

Load specific playlist:

```sh
mpc load <playlist_name>
```

Add a song into playlist:

```sh
mpc addplaylist <playlist_name> <song_name>
```

> [!TIP]
>
> Playlists are saved as files that is located in `~/.config/mpd/playlists`, you can append song's names to the playlist file to add the songs into the playlist.

