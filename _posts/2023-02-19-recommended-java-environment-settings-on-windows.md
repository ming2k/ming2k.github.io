---
title: recommended java environment settings on windows
date: 2023-02-19
---

# Windows 下推荐的 Java 环境设置

现在 Java 开发项目编码一般都是 UTF-8，Windows 下 JDK 的默认编码是 GB2312，可把其默认值设置为 UTF-8：

右键点击计算机 > 属性 > 高级系统设置 > 高级 > 环境变量 > 系统变量: 设置 JAVA_TOOL_OPTIONS 的值为 -Dfile.encoding=UTF-8

命令行显示 UTF-8 字符: 

执行 chcp 65001，设置命令行的属性，选择字体 Lucida Console(不要选择点阵字体)。如果要换回 GBK 执行 chcp 936 ，再把字体改成点阵字体即可。

如果使用 Intellij IDEA 的话，应该配置如下：

Help > Edit custom VM options 

添加 IDEA 的启动参数 `-Dfile.encoding=UTF-8`

同时配置 `Editor > File Encodings` 所有的编码为 `UTF-8 (without BOM)`

同时配置 `Editort > General > Console` 的编码为 `UTF-8`

