---
title: makefile tutorial
date: 2024-08-21
---

# Makefile tutorial



## 自动化变量

**`$@`**
代表当前规则的目标文件。例如，在一个规则 `target: dependency` 中，`$@` 就是 `target`。

**`$<`**
代表第一个依赖文件。例如，在规则 `target: dependency` 中，`$<` 就是 `dependency`。

**`$^`**
代表所有依赖文件的列表（没有重复）。在规则 `target: dependency1 dependency2` 中，`$^` 就是 `dependency1 dependency2`。

**`$?`**
代表所有比目标文件更新的依赖文件的列表。如果 `dependency1` 比 `target` 更新，`$?` 就是 `dependency1`。

**`$\*`**
代表目标文件名的主体部分，不包括扩展名。通常在模式规则中使用。

**`$(@D)`**
代表目标文件的目录部分。例如，如果目标文件是 `dir/file.txt`，则 `$(@D)` 就是 `dir`。

**`$(@F)`**
代表目标文件的文件名部分。例如，如果目标文件是 `dir/file.txt`，则 `$(@F)` 就是 `file.txt`

## 指令

1. **`all`**
   通常定义一个默认的目标，当你只运行 `make` 时，这个目标会被构建。例如：

   ```
   makefile
   Copy code
   all: target1 target2
   ```

2. **`clean`**
   用于清理构建产生的中间文件和目标文件。通常定义一个规则来删除这些文件：

   ```
   makefileCopy codeclean:
       rm -f *.o target1 target2
   ```

3. **`install`**
   用于将构建的文件安装到系统中的特定位置。这个规则会定义如何将文件复制或移动到目标目录：

   ```
   makefileCopy codeinstall: target1
       cp target1 /usr/local/bin/
   ```

4. **`PHONY`**
   用于声明伪目标，避免与实际文件名冲突。例如：

   ```
   makefile
   Copy code
   .PHONY: all clean install
   ```

5. **`$(MAKE)`**
   用于在规则中调用 `make` 自身，支持传递参数或变量。例如：

   ```
   makefileCopy codesubdir:
       $(MAKE) -C subdir
   ```

这些变量和指令有助于提高 Makefile 的灵活性和可维护性，使得构建过程更加高效和自动化。