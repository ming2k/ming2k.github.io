---
title: libratbag tutorial
date: 2024-02-02
---

# Libratbag Tutorial

## Install

### Arch

```sh
sudo pacman -S libratbag
sudo systemctl enable ratbagd
sudo systemctl restart ratbagd
```

### Gentoo

```sh
sudo emerge -a dev-libs/libratbag
sudo rc-update add ratbagd default
sudo rc-service ratbagd start
```

## Usage

list devices:

```sh
ratbagctl list
```

show current dpi

```sh
ratbagctl "<devcie_name>" dpi get
# for example
ratbagctl "Logitech G304" dpi get
```

set dpi

```sh
ratbagctl "<device_name>" dpi set <number>
# for example
ratbagctl "Logitech G304" dpi set 800
```

