---
title: rust tutorial
date: 2023-07-27
---

# Rust Tutorial

To begin our hacking journey, we need to set up Rust. The official Rust documentation recommends using `rustup` which includes . For more details, check [this guide](https://www.rust-lang.org/tools/install).

## Install Rust

Installing rust toolchains though `Rustup` is more recommended.

*Visual Studio Code may not be able to link the macro and other function source code unless you install Rust toolchains through rustup.*

## Rust Language Overview

We call the action of creating a reference `borrowing`. 

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

### Use Cargo.toml file


