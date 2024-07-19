---
title: guide to write gentoo ebuilds
date: 2024-05-10
---

# Gentoo Ebuilds 指南

refers:

- [Basic Guide to Write Gentoo Ebuilds - Gentoo](https://wiki.gentoo.org/wiki/Basic_guide_to_write_Gentoo_Ebuilds/zh-cn)
- [Creating an ebuild repository - Gentoo](https://wiki.gentoo.org/wiki/Creating_an_ebuild_repository)
- [](https://devmanual.gentoo.org/ebuild-writing/variables)

```sh
sudo pkgdev manifest -m
ebuild /path/to/hifetch-1.0.0.ebuild manifest
```

Creata an the ebuild repo by `eselect repository` from `eselect-repository` package:

```sh
eselect repository create <repository_name>
```

It will generate a directory lacated `/var/db/repos/<repository_name>`.

> [!TIPS]
> duo portage network sanbox, we can't download package during install.
> so we need offline compile program, such as `go project`
