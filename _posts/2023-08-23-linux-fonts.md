---
title: linux font tutorial
date: 2023-08-23
---

# Linux Fonts

该文章致力于教大家如何配置Linux操作系统的文字环境。

## 字体概述

Linux操作系统默认可以显示 ASCII 编码的所有字符，由于不同的发行版内置的字体文件不同，其对字体支持程度不同，对于 `Arch` 、 `Gentoo` 这种非完全的开箱即用的发行版，如果不手动处理的话会出现非 ASCII 编码的问题呈现为方块字的情况。

让我们探究方块字出现的背后的逻辑：本质上计算机是通过 `Unicode` 来区别不同文字的，Unicode 是一组列表用于容纳所有字形（glyphs），列表的特定编号集合被约束用于指定字形，比如 `U+54C8` 被分配给“哈”，虽然字体被指定但是图像的样式可以不同，比如是宋体或楷体。当计算机识别到 Unicode 后，如果有指定 Family 则去指定的 Family 中寻找该 Unicode 对应的字形是否存在，如果 Family 或存在或者 Family存在但对应的 Unicode 的字形为定义，则检索系统中安装的其他的 Family ，直到找到。

*每个字体文件通常会包含一个 `family`，`family` 为一组 `Unicode` 的图像集合，特定的字体通常用其 `family` 称呼。*

## 字体样式

在 `family` 中通常会包含 `serif`、`sans`、`mono`等关键字，比如 `Noto Serif Ethiopic`，本质是表示字体的样式。

| 英文               | 中文     | 描述 |
| ------------------ | -------- | ---- |
| serif              | 衬线字   |      |
| sans serif（sans） | 无衬线字 |      |
| monospace（mono）  | 等宽字   |      |
| Italic             | 斜体字   |      |

## 字体粗细

在静态的字体文件中的 `family` 名称会包含多种 `light`、`normal`、`bold` 等关键字，预先定义了多种粗细的样式，比如 `Noto Sans Ethiopic Thin`。值得一提的是如今的字体可以通过变形来实现粗细的变化，这种字体被称作 `Variable Font`。

*如果字体家族只包含一个字体文件（通常是常规或正常权重的字体），而没有专门的粗体（Bold）字体文件，Microsoft Word等文字处理软件会尝试通过字形变换的方式来模拟粗体效果。这个过程被称为“伪粗体”（Fake Bold）。*

但还是让我们了解这些预先设定的字体的粗细（weight）的含义：

| 术语                                              | 描述                                       |
| ------------------------------------------------- | ------------------------------------------ |
| Thin（纤细）/Hairline（细线）/Ultra Light（极轻） | 非常细的字体，适用于一些特殊设计。         |
| Light（轻）/Extra Light（额外轻）                 | 较为轻盈，适用于一些柔和感觉的设计。       |
| Regular（正常）/Normal（普通）/Book（书体）       | 正常的字体权重，通常是默认文本样式。       |
| Medium（中等）                                    | 介于正常和加粗之间，提供中等的粗细程度。   |
| Semi-bold（半粗体）/Demi-bold（半粗体）           | 比正常稍微加粗一些，但没有达到加粗的程度。 |
| Bold（粗体）                                      | 明显比正常字体更粗，用于强调文本或标题。   |
| Extra Bold（极粗体）/Ultra Bold（超粗体）         | 较为粗重，常用于需要强烈突出的场合。       |
| Black（黑体）/Heavy（重体）                       | 最粗的字体，提供最强烈的突出效果。         |

## 安装字体

### 手动安装（推荐）

1. 下载字体文件；

2. 将字体文件放在指定目录：

   - 系统目录：`/usr/share/fonts/`
   - 用户目录：`~/.fonts/`

   推荐将字体文件安装在用户目录，方便数据迁移和保证全局的纯净；

3. 刷新字体缓存

   ```sh
   # 更新系统字体缓存
   sudo fc-cache -f -v
   ```

### 包管理工具安装

```sh
# arch linux
sudo pacman -S adobe-source-han-sans-otc-fonts noto-fonts-cjk 
```

## 字体管理

查看字体

```sh
fc-list
# 产看中文的的字体列表
fc-list :lang=zh
fc-list :file=/usr/share/fonts/adobe-source-han-sans/SourceHanSans.ttc
fc-list :family
```

更新字体缓存

```sh
# 更新系统字体缓存
sudo fc-cache -f -v
```

## 字体配置

*配置用户手册`/usr/share/doc/fontconfig/fontconfig-user.html`*

有两处配置字体的配置文件：

- 系统的配置，**不推荐修改**：`/etc/fonts/fonts.conf`
- 用户的配置：`~/.config/fontconfig/fonts.conf`

### 个人配置推荐

`~/.config/fontconfig/fonts.conf`

**确保下属字体存在或者名字正确，使用 `fc-lust` 查看**

```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "urn:fontconfig:fonts.dtd">
<fontconfig>
  	<alias>
	    <family>sans-serif</family>
	    <prefer>
		    <family>Source Han Sans SC</family>
	    </prefer>
    </alias>
  	<alias>
	    <family>serif</family>
	    <prefer>
		    <family>Noto Serif CJK SC</family>
	    </prefer>
    </alias>
    <alias>
	    <family>sans</family>
	    <prefer>
		    <family>Source Han Sans SC</family>
	    </prefer>
     </alias>
    <alias>
	    <family>monospace</family>
	    <prefer>
		    <family>Noto Sans Mono CJK SC</family>
	    </prefer>
    </alias>
</fontconfig>
```

Chrome和electron会遵守 `sans-serif` 设置字体，所以这个是需要设置的。

## 字体推荐

安装字体的话推荐 [Google Noto 字体](https://fonts.google.com/) ，开源免费且字体覆盖全面。

| 字体家族            | 描述                                    |
| ------------------- | --------------------------------------- |
| Noto Emoji          | 无颜色的 Emoji                          |
| Noto Color Emoji    | 有颜色的 Emoji                          |
| Noto Sans Symbols 2 | 为 Noto Sans Symbols 的第二版，覆盖更全 |
| Noto Serif SC       | 简体中文                                |
| Noto Serif TC       | 繁体中文                                |

使用 Noto 系列的字体可以保证样式的一致性。

除了 `Emoji` 和 `Simbols` 这两个特殊字库，其他的我们要安装的字库多为区域性文字字库，比如 `简中` 和 `繁中`。通常你可以使用 [Google Maps](https://maps.google.com/maps) 通过查看地名（一般会使用当地过来的语言展示），如果为方块字你可以查询当地所用的语言，安装指定的 Noto 字库。

此外还推荐其他的字体：

| 字体家族                  | 开源 | 描述                                             |
| ------------------------- | ---- | ------------------------------------------------ |
| DejaVu                    | y    | 字库比较全面                                     |
| TeX Gyre                  | y    | 字库比较全面                                     |
| Souce han sans            | y    | 支持 CJK，Adobe 设计公司背书                     |
| Liberation Serif          | y    | 用于替代 Times New Roman 的衬线字体              |
| Liberation Sans           | y    | 用于替代 Arial 的无衬线字体                      |
| Liberation Mono           | y    | 用于替代 Courier New 的等宽字体                  |
| google-crosextra Carlito  | y    | 是对 Microsoft 的 Calibri 字体的自由替代品       |
| google-crosextra  Caladea | y    | 是对 Microsoft 的 Cambria 字体的替代品           |
| fira code                 | y    | 由 Mozilla 开发，用于编程，支持连字（ligatures） |

## 其他

当你遇到方块字的时候，可以通过[Global Fonts](https://globalfonts.pro)查询对应的字体 font family。

举例，使用该语句搜索 `"<方块字>" noto font site:globalfonts.pro` 会通过 `Global Fonts` 查询包含该方块字存在的 Noto 系列字体。
