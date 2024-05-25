---
title: "Getting started with Rust development"
datePublished: Mon Feb 06 2023 17:20:41 GMT+0000 (Coordinated Universal Time)
cuid: cldt2x5it000009js737od9uf
slug: rust-series-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1716643494748/7d385447-dd56-4725-9b1c-f7cbf43ba2d5.gif
tags: learning, rust, getting-started, rust-lang, rustseries

---

Welcome to the course. Don't forget to subscribe to my newsletter for regular updates.

Before I begin I have to mention that I am using a Mac machine(which is very similar to a Linux machine), which means the commands that I will be showing you are for Linux and Mac systems (UNIX-based machines). If you are using a windows machine just google how to do it in windows or refer to the [rust book](https://doc.rust-lang.org/book/).

*Note: The code will not be different in a windows machine just the commands and some configurations will be different.*

### GitHub repo with all the code

[https://github.com/codetit4n/rust-school](https://github.com/codetit4n/rust-school)

Make sure to star/fork/watch it on GitHub.

### Objectives of this lesson

Setting up rust and Writing your first program.

### Installation

Now, I won't bore you with commands here. Just refer to this link: [https://doc.rust-lang.org/book/ch01-01-installation.html](https://doc.rust-lang.org/book/ch01-01-installation.html) for installation guidelines as per your system.

A few things to note here:

What is [rustup](https://rustup.rs/)? It is the official rust toolchain installer. Used to manage rust on your system. It can be used to install, [update and uninstall](https://doc.rust-lang.org/book/ch01-01-installation.html#updating-and-uninstalling) rust on your system. This is an amazing tool because not only it can manage the rust versions but it also comes with an offline version of the [rust book](https://doc.rust-lang.org/book/). For that, just run: `rustup docs --book`

### Your first rust program

Rust files will have the extension ".rs". Now, let's write our first program. Make a folder and in that folder make a file called `hello.rs`

Inside the hello.rs file:

```rust
fn main(){
    println!("Hello rust!");
}
```

I will walk you through the code in some time.

Now, Since rust is a compiled language. Which means we have to first compile the code in order to make it work. And we will use the rust compiler for it. The compiler is called `rustc`

To compile in the terminal I'll run:

```bash
rustc hello.rs
```

This will make an executable file by the same name in the folder:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675700588523/88e6903c-cc96-40a0-a8bc-5e991bd37281.png align="left")

Here, you can see in my terminal the orange-colored one is the executable file. In a windows system, it should be something like `hello.exe`

Now, this file contains all the machine-level instructions to make our program work. To run this I can execute this file using:

```bash
./hello
```

Which will give me:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675701046883/e8b7976f-b022-47d6-99c8-fb211966b928.png align="left")

There you go. You can see it printed "Hello rust!". Congratulations you just wrote your first rust program.

### Code walkthrough

Now, let's go over the code to understand what is happening:

```rust
fn main(){
    println!("Hello rust!");
}
```

If you are familiar with languages like C and C++ you will get it. If not let me explain:

* fn is a keyword. Think of keywords as reserved words that have special meanings. We will go deep into keywords in the coming lessons.
    
* `main()` is the name of the function. Now, `main()` is a special function, which serves as an entry point to the program. It tells the compiler to start compiling the code from here. Note: All functions will have `()` in front of them.
    
* `println!()` may look like a function but it is not. It is a macro. You can identify a micro by the `!` in front of the name. Macros provide some special functionality, for instance here the macro `println!()` prints a string to the screen.
    
* The `{}` is called a [block expression](https://doc.rust-lang.org/reference/expressions/block-expr.html). Ignore it for now, we will go deep into it later.
    
* We end the `println!()` statement with a semicolon(`;`) which indicates that this expression is over and the next one is ready to begin.
    

So, this was a very basic program that just prints a simple thing on the screen.

Code: [https://github.com/codetit4n/rust-school/tree/main/lesson-1](https://github.com/codetit4n/rust-school/tree/main/lesson-1)

*Make sure to play with it and experiment*.

### Few conventions to note

1. It is a convention to use the name main.rs for your main rust file.
    
2. The rust community prefers to use 4 spaces instead of a tab (6 spaces).
    
3. If you want to give a multi-word name to your rust file, the convention is to use an underscore to separate the words. Examples: `hello_rust.rs` , `i_like_rust.rs`
    

Note: You are not obliged to use these conventions. These are just suggestions.

### P.S.

I know some of the readers might feel ("especially those with a lot of experience programming") that going to so many details in just a hello world lesson is useless. But bear with me I will pick up the pace. And if you have any feedback, suggestions, doubts, etc feel free to reach out to me.