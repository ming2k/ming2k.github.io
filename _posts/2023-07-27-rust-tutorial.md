---
title: rust tutorial
date: 2023-07-27
---

# Rust Tutorial

To begin our hacking journey, we need to set up Rust. The official Rust documentation recommends using `rustup` which includes . For more details, check [this guide](https://www.rust-lang.org/tools/install).

## Get Started with a Simple Example

1. Select an appropriate workspace, create a file named `main.rs`,  and add the following code to the file:

    ```rust
    fn main() {
        println!("Hello, world!");
    }
    ```

2. Compile the above mentioned files:

    ```sh
    rustc main.rs
    ```

    `rustc` will generate a file with the same base name, also known as the filename stem, as the compiled file.

3. Run the executable file:

    ```sh
    ./main
    # for Windows
    ./mian.exe
    ```

    The Console will print `Hello, world!`.

## Using `Cargo` to Manage Your Project

`Cargo` is the package manager and build system for Rust. So you should know the basic commands if you want to mantain the project systematically.

### Simple and Useful Commands 

Create a new project with scaffold: 

```sh
cargo new <your_project_name>
```

Run your project: 

```sh
cargo run
```

## Choosing an IDE



推荐使用 VSCode + WSL 开发，以下针对 VSCode + WSL 环境配置做介绍。

VSCode 插件推荐：

**WSL**

VSCode 使用 WSL 作为开发环境的工具，必备。 

**rust-analyzer**:

Rust 分析工具，必备。

### 预先引入

像其他语言一样，Rust 预先引入了常用类型、traints 以及常用宏，详情可见 [prelude官方文档](https://doc.rust-lang.org/std/prelude/index.html) 。

```rust
use std::prelude::rust_2018;
```

也可以利用IDE进行跳转源码，查看预编译的类型。

在翻阅源码的时候我发现，除了`std` 包含 `prelude` 之外， `core` 也包含，那么它两者是什么关系呢？ 

`std` 建立在 `core` 基础之上。

默认引入 `std` 而不是 `core` 。因为 `std` 比 `core` 的 `prelude`  多 `Vec` ，实际开发中 `Vec` 是不需要显性引入。

### 数据结构

clone