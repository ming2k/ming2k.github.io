---
title: guide to write gentoo ebuilds
date: 2024-05-10
---

# Gentoo Ebuilds 指南

refers:

- [Basic Guide to Write Gentoo Ebuilds - Gentoo](https://wiki.gentoo.org/wiki/Basic_guide_to_write_Gentoo_Ebuilds/zh-cn)
- [Creating an ebuild repository - Gentoo](https://wiki.gentoo.org/wiki/Creating_an_ebuild_repository)
- [](https://devmanual.gentoo.org/ebuild-writing/variables)

## Create Repo

Creata an the ebuild repo by `eselect repository` from `eselect-repository` package:

```sh
eselect repository create <repository_name>
```

It will generate a directory lacated `/var/db/repos/<repository_name>`.

> [!TIPS]
> duo portage network sanbox, we can't download package during install.
> so we need offline compile program, such as `go project`


## Build Manifest

```sh
sudo pkgdev manifest -m
GENTOO_MIRRORS="" ebuild /path/to/hifetch-1.0.0.ebuild manifest clean unpack
```

## Install

Test Install

```sh
ebuild scrub-2.6.1.ebuild clean test install
```

Merge

```sh
ebuild scrub-2.6.1.ebuild clean install merge
```
