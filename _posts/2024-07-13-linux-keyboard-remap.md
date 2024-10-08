---
title: linux keyboard remap
date: 2024-07-13
---

# Linux Keyboard Remap

## TL;DR

1. Create the hwdb config file in `/etc/udev/hwdb.d`, for example:

   /etc/udev/hwdb.d/90-custom-keyboard.hwdb

   ```hwdb
   evdev:atkbd:*
    KEYBOARD_KEY_<scancode>=<keycode>
    # ...
   ```

2. xxxxxxxxxx sudo systemctl restart getty@tty1.servicesh

   ```sh
   systemd-hwdb update
   ```

3. Reloading the Hardware Database Index

   ```sh
   udevadm trigger
   ```

refer:

1. https://wiki.archlinux.org/title/map_scancodes_to_keycodes
2. https://wiki.archlinux.org/title/Keyboard_input

## How to Write the hwdb Config

for example:

`/etc/udev/hwdb.d/90-custom-keyboard.hwdb`:

```hwdb
evdev:input:b0003v04D9p4545e0110*
 KEYBOARD_KEY_700e2=leftmeta
 KEYBOARD_KEY_700e3=leftalt
```

### How to Confrim Your Device Module

`module` follows `evdev`, for example `evdev:input:xxxx`

1. Comfrim which event is the module that you want to check

  Use the following command can show all events:

  ```sh
  evtest
  ```

2. Using the event to query module string, load the appropriate kernel module for the device.

   Execute the following to check more detial:

   ```sh
   cat /sys/class/input/eventX/device/modalias
   ```

3. Use content of the modalias output correctly.

   for example output:

   ```log
   input:b0011v0001p0001eAB83-e0,1,4,11,14,k71,72,73,74,75,76,77,79,7A,7B,7C,7D,7E,7F,80,8C,8E,8F,9B,9C,9D,9E,9F,A3,A4,A5,A6,AC,AD,B7,B8,B9,D9,E2,ram4,l0,1,2,sfw
   ```

   You can edit as following:

   ```hwdb
   evdev:input:b0011v0001p0001eAB83*
   ```

   `b` means `bus`, `v` means vendor, `p` means product, `e` means `version`.

#### How to Confrim Scancode and Keycode

Use the following command can check scancode and scancode's keycode map:

```sh
evtest /dev/input/event<X>
# X is number
```

Press the key to trigger event:

Analyze logs, for exmaple:

```log
Event: time 1725605974.103610, type 4 (EV_MSC), code 4 (MSC_SCAN), value db
Event: time 1725605974.103610, type 1 (EV_KEY), code 56 (KEY_LEFTALT), value 0
Event: time 1725605974.103610, -------------- SYN_REPORT ------------
```

`scancode` is `db`
`keycode` is `56/left alt`

Besides, `/usr/include/linux/input-event-codes.h` contains the keycode, see the `KEY_<KEYCODE>` variables. for example `leftalt`.

## hwdb Config Example

### Swap ALT WITH SUPER AKA. META

```hwdb
evdev:atkbd:*
 keyboard_key_38=leftmeta
 keyboard_key_db=leftalt
```
