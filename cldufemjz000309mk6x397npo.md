---
title: "Cargo Basics"
datePublished: Tue Feb 07 2023 15:57:58 GMT+0000 (Coordinated Universal Time)
cuid: cldufemjz000309mk6x397npo
slug: rust-series-2
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1716643702911/9366b97d-2743-4ad1-a1ea-98a224084ba8.gif
tags: learning, rust, rust-lang, cargotoml, rustseries

---

### GitHub repo with all the code

[https://github.com/codetit4n/rust-school](https://github.com/codetit4n/rust-school)

Make sure to star/fork/watch it on GitHub.

### What is Cargo?

Cargo is Rust’s build system and package manager. If you are familiar with `npm` from the Node.Js ecosystem, you can think of cargo as something similar for the rust ecosystem.

### What can Cargo do?

As you write complex rust programs with a lot of dependencies you need a tool to manage everything. You can't rely on `rustc` compiler to compile and manage all your dependencies. With Cargo, you can manage multiple dependencies very efficiently.

### Similarities between Cargo and npm

* Like npm's `package.json` we have `Cargo.toml` for rust projects.
    
* In npm, we install packages using `npm install package` similarly, in rust, we can install crates (packages are called crates in the rust ecosystem) using `cargo install crate`
    

### Setup Cargo

Cargo comes installed with Rust if you used the official installers.

To check your cargo version you can do: `cargo --version`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675775216315/b27d5bc4-b658-466e-aac2-198b9d0e1130.png align="center")

If it does not show your cargo version, that would mean cargo is not installed properly in your system.

### Creating a new project with Cargo

To create a new project we will do:

```bash
cargo new first_cargo_proj
```

This will create a new cargo project called first\_cargo\_proj:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675775423732/c9cd9350-bcbf-4079-8b3b-ee185b807fea.png align="center")

The cargo projects are called packages. Think of a package as a collection of source files and a `Cargo.toml` file that describes the package. (Like we have the `package.json` in Node.Js projects).

The folder structure created by Cargo:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675775681238/fa8e5659-31ac-4843-a4ed-d212191f6843.png align="left")

Note: If you create a new project outside a git repository it will come with the git configurations like `.gitignore` file.

In the folder structure, we have:

1. src folder which will contain all the rust files. You can see the `main.rs` here.
    
2. Then we have the `Cargo.toml` file.
    

### What is the Cargo.toml file?

Cargo.toml is a manifest file. Which means it contains the description of the package. In rust, we use the TOML language for manifests. TOML stands for "Tom's Obvious Minimal Language". Checkout: [https://toml.io](https://toml.io/)

### Understanding the Cargo.toml file

Now, let's look into the Cargo.toml file:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675777058365/8d785741-23aa-4a71-abdf-9b1d44b7500f.png align="center")

The first line, `[package]`, is a section heading that indicates that the following statements are configuring a package.

The `[package]` section contains configurations Cargo needs to compile your program, like the name, version, and the edition of rust to use.

The last line `[dependencies]` is the section which will contain all the dependencies of the package. These dependencies are called crates. You can browse these crates on [https://crates.io](https://crates.io/)

### Using Cargo

First, let's take a look at the `src/main.rs`:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675778234648/a6f36eb0-e412-45d3-8ab0-d3514ab2abfd.png align="left")

You can see rust generated a new Hello World program for us. Now, let's build and run this using Cargo.

To build the project you can do: `cargo build`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675778345453/fa518931-f63d-4b6e-81b6-7241a8b7963d.png align="center")

This will compile and build the project. Earlier we were doing it using `rustc`.

Now, if I check the directory I will find a new target folder:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675784510309/b1865170-c454-4303-ac3b-3b59977788c2.png align="left")

And in the target folder, I have a new debug folder:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675778510795/7b0c6b03-2196-40e5-8e88-d44e631ac30c.png align="left")

This folder contains the executable file to run the program. You can see the executable file here, it is called `first_cargo_proj`(`first_cargo_proj.exe` for windows).

Refer: [https://github.com/codetit4n/rust-school/tree/main/lesson-2/first\_cargo\_proj/target](https://github.com/codetit4n/rust-school/tree/main/lesson-2/first_cargo_proj/target)

*NOTE: I have pushed the target folder in the repo just for reference. Ideally, it should not be pushed to git repos.*

Now, I can run this executable file using: `./target/debug/first_cargo_proj`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675778674749/0951c8f2-7b1f-4cd2-a2f2-d08a229fad01.png align="left")

You can see it worked! But there is a better way. You can just do: `cargo run`

This will build and run your project in one command.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675778766810/1a46e7d0-10c8-4a5e-8871-831b71b3e062.png align="center")

Notice that this time we didn’t see output indicating that Cargo was compiling `first_cargo_proj`. Cargo figured out that the files hadn’t changed, so it didn’t rebuild but just ran the binary (executable file).

One more thing, sometimes it can be the case that you do not want to build your project and just want to check the program for errors. This happens a lot, especially in big projects. For that you can do: `cargo check` . This command quickly checks your code to make sure it compiles but doesn’t produce an executable. Plus you don't want to build your entire project when doing small changes to check if your code works. So, make sure to use this command.

### Building your rust code for a release

Now, by default when you do `cargo build` it will create a debug folder in the target folder which will have the executable. But, this is not the optimized version. It is for development and debugging purposes only.

To make a production build you should run `cargo build --release` to compile with optimizations. The optimizations make your Rust code run faster, but turning them on lengthens the time it takes for your program to compile. Running this will make a release folder inside the target folder with the executable file:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675785087056/df405d86-13b9-4811-bba2-06dbb0da80fd.png align="left")

### Some interesting things to note

* Cargo has an entire book written for it called "[the cargo book](https://doc.rust-lang.org/cargo/index.html)".
    
* With simple projects, Cargo doesn’t provide a lot of value over just using `rustc`, but it will prove its worth as your programs become more intricate. Once programs grow to multiple files or need a dependency, it’s much easier to let Cargo coordinate the build.
    
* TOML aims to be a minimal configuration file format that's easy to read due to obvious semantics.
    

### P.S.

There is a lot to learn about cargo. But as a beginner, this should suffice.