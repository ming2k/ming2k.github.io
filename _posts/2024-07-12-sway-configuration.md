title: sway configuration
date: 2024-07-12

---

# sway configuration

## Windows Behaviors

```sh
for_window [app_id="ncmpcpp"] floating enable
```

Get windows properties:

```sh
swaymsg -t get_tree
```

## TTY1 auto login

1. Auto exec sway command

   edit `/etc/profile`, append the following content in the file:

   ```sh
   if [ -z $DISPLAY ] && [ "$(tty)" = "/dev/tty1" ]; then
     exec sway
   fi
   ```

2. Autologin into your system

   ```sh
   sudo systemctl edit getty@tty1
   ```

   append the following content:

   ```
   [Service]
   ExecStart=
   ExecStart=-/sbin/agetty --autologin joe --noclear %I 38400 linux
   ```

## Utils

### screenshot

packages: grim + slurp

cmd:

```conf
bindsym Print exec grim -g "$(slurp -d)" - | wl-copy
```

## Error

[error] Error calling StartServiceByName for org.freedesktop.portal.Desktop: Timeout was reached

uninstall `xdg-desktop-portal-gtk` and mask `xdg-desktop-portal-wlr`

```sh
sudo emerge -C xdg-desktop-portal-gtk
systemctl --user mask xdg-desktop-portal-wlr
```
