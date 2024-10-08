---
title: flatpak tutorial
date: 2024-04-29
---

# flatpak tutorial

Flatpak uses Bwrap to create isolated environments (sandboxes) where applications can run securely without affecting the rest of the system.

## Basic Comamnds

应用安装的位置：`/var/lib/flatpak`

更新：

```sh
flatpak update
```

卸载：

```sh
flatpak uninstall --unused
```

列举：

```sh
# 列出相应一列
flatpak list --columns=ref
# 只列出app
flatpak list --app
```

## XDG Desktop Portal

XDG desktop portal is a freedesktop.org project that provides a service for desktop applications to access various desktop APIs in a sandboxed environment. It acts as a bridge between sandboxed applications (like those in Flatpak) and the host system's desktop environment.

It means that the sandbox must access the host by the portal.

The following is the manual of the portal:

```sh
man portals.conf
```

For gentoo, the system-wide config is located in `/usr/share/xdg-desktop-portal/`.

To make it work in your current desktop environment, you need to set `XDG_CURRENT_DESKTOP` environment, and let systemd know it.


```sh
dbus-update-activation-environment --systemd XDG_CURRENT_DESKTOP
# This command imports the system environment into dbus  and systemd. Ensure the system has set the variable, such as `export XDG_CURRENT_DESKTOP=sway`
```

The xdg-desktop-portal interface requires specific backends. For sway users, I recommend using xdg-desktop-portal-gtk for general purposes and xdg-desktop-portal-wlr specifically for screencasting.

After we donwload the portal backends, we will find `/usr/share/xdg-desktop-portal/portals/gtk.portal` and `/usr/share/xdg-desktop-portal/portals/wlr.portal`, their content format is like:

```sh
[portal]
DBusName=org.freedesktop.impl.portal.desktop.gtk
Interfaces=org.freedesktop.impl.portal.Settings;org.freedesktop.impl.portal.FileChooser;org.freedesktop.impl.portal.AppChooser;org.freedesktop.impl.portal.Print;org.freedesktop.impl.portal.Notification;org.freedesktop.impl.portal.Inhibit;org.freedesktop.impl.portal.Access;org.freedesktop.impl.portal.Account;org.freedesktop.impl.portal.Email;org.freedesktop.impl.portal.DynamicLauncher;org.freedesktop.impl.portal.Settings;
UseIn=gnome
```

We can change the content to meet our requirements, for example, we can change the `UsedIn` value to make it work on `sway`:

```sh
[portal]
DBusName=org.freedesktop.impl.portal.desktop.gtk
Interfaces=org.freedesktop.impl.portal.Settings;org.freedesktop.impl.portal.FileChooser;org.freedesktop.impl.portal.AppChooser;org.freedesktop.impl.portal.Print;org.freedesktop.impl.portal.Notification;org.freedesktop.impl.portal.Inhibit;org.freedesktop.impl.portal.Access;org.freedesktop.impl.portal.Account;org.freedesktop.impl.portal.Email;org.freedesktop.impl.portal.DynamicLauncher;org.freedesktop.impl.portal.Settings;
UseIn=gnome;sway;
```

After we config the configs of the backends, we can use the backends in xdg-desktop-portal, user-wide config will override the system-wide config.

For user-wide we can set `~/.config/xdg-desktop-portal/{desktop-}portals.conf`, for sway, it is `~/.config/xdg-desktop-portal/sway-portals.conf`

```
[preferred]
default=*
org.freedesktop.impl.portal.ScreenCast=wlr
org.freedesktop.impl.portal.Screenshot=wlr
```

the value can choise the backends, gtk or wlr.











