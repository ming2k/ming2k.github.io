title: linux keyboard remap
date: 2024-07-13

---

# Linux Keyboard Remap

## TL;DR

1. Create the hwdb config file, for example:

   /etc/udev/hwdb.d/90-custom-keyboard.hwdb

   ```hwdb
   evdev:atkbd:*
    KEYBOARD_KEY_<scancode>=<keycode>
    # ...
   ```

2. Updating the Hardware Database Index

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

## Learn How to Write the hwdb Config

### Specify the Kernel Module

`module` follows `evdev`, for example `evdev:input:xxxx`

#### How to Confrim Your Device Module

1. Comfrim which event is the module that you want to check

   Use the following command can reveal more detials(driver version, ID, name etc):

   ```sh
   evtest /dev/input/event<X>
   # X is number
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

#### How to Confrim Scancode and Keycode

`/usr/include/linux/input-event-codes.h` contains the keycode, see the `KEY_<KEYCODE>` variables. for example `leftalt`.

Use the following command can check scancode and scancode's keycode map:

```sh
evtest /dev/input/event<X>
# X is number
```

## hwdb Config Example

### Swap ALT WITH SUPER AKA. META

```hwdb
evdev:atkbd:*
 KEYBOARD_KEY_38=leftmeta
 KEYBOARD_KEY_db=leftalt
```
