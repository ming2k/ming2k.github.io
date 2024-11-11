---
title: setting linux desktop appearance
date: 2024-07-15
---

# Setting Linux Desktop Appearance

ref:

- [Icons - Arch Wiki](https://wiki.archlinux.org/title/icons)
- [GTK - Arch Wiki](https://wiki.archlinux.org/title/GTK)
- [FreeDesktop icon theme spec](https://specifications.freedesktop.org/icon-theme-spec)

## Icon Setting

Appearance includes `icon` and `gtk-theme`.

Icon is included in `FreeDesktop Icon Theme Specification`, that means it's generally usable.

the icon path:

1. `/usr/share/icons/`
2. `~/.icons`
3. `~/.local/share/icons/`

### Configure Cursor

1. Download Cursor file to icon path.

2. Setting gsettings set cursor theme, but it only work for gtk program

  ```sh
  gsettings set org.gnome.desktop.interface cursor-theme '<THEME_NAME>'
  ```
3. For it work on all appliaction, config sway configure:

  ```sh
  seat seat0 xcursor_theme <THEME_NAME> 24
  ```


## Theme Setting

`gtk-theme` configure the gtk style and behavior, it is only for gtk applications.

the theme path:

1. `/usr/share/themes/`
2. `~/.themes`
3. `~/.local/share/themes/`

If theme not include `gtk` config, you can config them in `~/.config/gtk-3.0/`, `~/.config/gtk-4.0/` etc.

## Specify the Theme

For GTK applications, you can specify the `theme name` and `icon name` by `gsettings`.

### by System Environment

```sh
export GTK_THEME=Adwaita:dark
```

### by Gsettings

ref: [GTK4 Settings](https://docs.gtk.org/gtk4/class.Settings.html)

On the X window system, this sharing is realized by an XSettings manager that is usually part of the desktop environment, along with utilities that let the user change these settings.

On Wayland, the settings are obtained either via a settings portal, or by reading desktop settings from `GSettings`.

In the absence of these sharing mechanisms, GTK reads default values for settings from settings.ini files in /etc/gtk-4.0, `$XDG_CONFIG_DIRS/gtk-4.0` and `$XDG_CONFIG_HOME/gtk-4.0`.

1. install `gsettings-desktop-schemas`

2. set as the following:

   ```sh
   gsettings set org.gnome.desktop.interface gtk-theme 'Adwaita-dark'
   gsettings set org.gnome.desktop.interface icon-theme 'Adwaita-dark'
   gsettings set org.gnome.desktop.interface cursor-theme 'Adwaita'
   gsettings set org.gnome.desktop.interface font-name 'Noto Sans Nerd Font 12'
   ```

### by GTK Config file

```ini
[Settings]
gtk-theme-name=Adwaita-dark
gtk-application-prefer-dark-theme=1
gtk-button-images=true
gtk-cursor-theme-name=Adwaita
gtk-cursor-theme-size=24
gtk-decoration-layout=icon:minimize,maximize,close
gtk-enable-animations=true
gtk-font-name=Noto Sans, 10
gtk-icon-theme-name=Adwaita
gtk-menu-images=true
gtk-modules=colorreload-gtk-module:window-decorations-gtk-module
gtk-primary-button-warps-slider=false
gtk-toolbar-style=3
gtk-xft-dpi=196608
```

## Get Theme

GNOME Look

Deviant Art

Open Desktop

[Gnome Official Themes](https://gitlab.gnome.org/Archive/gnome-themes-extra/)
