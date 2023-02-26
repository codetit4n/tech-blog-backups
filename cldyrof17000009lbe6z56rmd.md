# Make a strong  Rust Foundation - Data Types, Variables, Mutability, and Constants

### Objective

There are some common concepts that appear in almost every programming language. Let's dive into them here to start your Rust journey right.

### Before we begin

I will try my best to go into as much detail as possible. But don't worry if you find some things hard to grasp. You can always come back after learning more or building some practice projects.

### GitHub repo with all the code

[**https://github.com/codeTIT4N/rust-school**](https://github.com/codeTIT4N/rust-school)

Make sure to star/fork/watch it on GitHub.

### Some system-level fundamentals you should know

Since we are getting started with rust, I won't go very deep into Rust's memory management in this article. I will cover that later with things like Stack, Heap, static memory, etc. But for now, you need to understand what memory is.

There are 2 types of memory:

* Persistent memory: Think of your HDD/SSD. It is the persistent memory because things you put on it can persist even after you turn off your computer. Or you can think of a database where you put some data, which will persist. This is slower compared to volatile memory and is more abundant.
    
* Volatile(temporary) memory: Think of your RAM(Random Access Memory). This is a memory that will be erased if you shut down your computer. This is faster than persistent memory and scarce. The important thing to note is that *whenever a program is running it uses this memory.*
    

Now, there are a lot of details that we can go into for memory but as a beginner, you should be familiar with at least this much.

Any memory in a computer will store only binary data (1s and 0s). However, anything can be represented in binary. Your program will determine what the binary represents.

### Data types

There are some fundamental data types that every language provides that are universally useful. Data types tell rust what kind of data is being specified so it knows how to work with that data.

So, what are some commonly used data types in rust?

* Boolean: true or false
    
* Integer: 1, 2, 30, -2,-40, etc
    
* Double/Float: 1.2, 10.4, etc
    
* Characters: 'A', 'B', 'C', etc
    
* String: "Hello Rust", "I am from the future", etc
    

Since Rust is a *statically typed* language, which means that it must know the types of all variables at compile time. The compiler can usually infer what type we want to use based on the value and how we use it. But sometimes we might have to tell the compiler explicitly. Now, let's learn about variables and we will revisit this after that.

### Variables

Humans can't directly work with memory(at least not easily), so we need variables to make that easier for us. A variable is a way to assign data to a temporary/volatile memory location.

NOTE: Here, the variables will be using the temporary memory(i.e., RAM).

### Mutability

Variables in Rust are by default immutable. Which means the values can't be changed once set. If you want to make a variable mutable you have to explicitly tell the Rust compiler. This is Rust's way of encouraging you to favor immutability. Consider this code snippet:

```rust
fn main() {
    let x = 1;
    println!("The value of x is: {x}");
}
```

This will simply print the value of x and you will get:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676026428303/6ac2497c-09f0-43f7-a9ba-169a7589c0ad.png align="left")

Here, by using the `let` statement we have declared a variable x and assigned it a value of 1. And after that, we are printing it. By default, this is an immutable variable. So, if I try to reassign it to another value:

```rust
fn main() {
    let x = 1;
    println!("The value of x is: {x}");
    x = 2; //will give error
}
```

It will give me an error:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676026741508/0ca25f0a-8ab8-4baa-95b3-43f5a67c8f99.png align="center")

NOTE: How beautifully the Rust compiler describes the error and possible fixes. This is another amazing reason to use Rust.

The reason for this error is that it cannot assign an immutable variable twice. Which is very self-explanatory.

But, I can fix this by making the variable x mutable like this:

```rust
fn main() {
    let mut y = 1; //mutable variable
    y = 4;
    println!("The value of y is: {y}");
}
```

Output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676027018185/bf37dea6-8ff6-4c9f-a069-56bd1c4ba4d5.png align="center")

Here, you can see it prints the correct values and does not give any errors. It is giving some warnings, but for now, we can ignore these warnings.

This is the way you can make your variable mutable by using `mut`.

### Revisiting Data Types and using Type Annotations

Now, consider:

```rust
let x = 4;
```

Here, the compiler can infer the data type. But there are certain cases where we need to specify the data type explicitly. Before going into that let's learn how are the different Data Types represented in Rust:

The Data Types in Rust can be broadly classified into 2 categories:

1. Scalar Types
    
2. Compound Types
    

**Scalar types**: These are types that represent a single value. Rust has four primary scalar types: integers, floating-point numbers, Booleans, and characters. We will learn about Compound types in Part 2 (next article).

**Integer types**:

An integer is a number without a fractional component. There is a very good table given in the [rust book](https://doc.rust-lang.org/book/ch03-02-data-types.html#integer-types):

| Length | Signed | Unsigned |
| --- | --- | --- |
| 8-bit | `i8` | `u8` |
| 16-bit | `i16` | `u16` |
| 32-bit | `i32` | `u32` |
| 64-bit | `i64` | `u64` |
| 128-bit | `i128` | `u128` |
| arch | `isize` | `usize` |

Signed includes negative numbers but unsigned do not.

Here, for example: If you want to represent a signed 8-bit number it will have the type `i8`. The unsigned version will be `u8`.

"Each signed variant can store numbers from -(2<sup>n - 1</sup>) to 2<sup>n - 1</sup> - 1 inclusive, where *n* is the number of bits that variant uses. So an `i8` can store numbers from -(2<sup>7</sup>) to 2<sup>7</sup> - 1, which equals -128 to 127. Unsigned variants can store numbers from 0 to 2<sup>n</sup> - 1, so a `u8` can store numbers from 0 to 2<sup>8</sup> - 1, which equals 0 to 255."

Side Note: In the computer system, the signed numbers are stored using a method called [two's complement](https://en.wikipedia.org/wiki/Two%27s_complement).

Additionally, the `isize` and `usize` types depend on the architecture of the computer your program is running on, which is denoted in the table as “arch”: 64 bits if you’re on a 64-bit architecture and 32 bits if you’re on a 32-bit architecture.

Few interesting ways to store integers:

You can use visual separators like `1_000`, which will mean `1000`. Number literals that can be multiple numeric types allow a type suffix, such as `57u8`, to designate the type.

So how do you know which type of integer to use? If you’re unsure, Rust’s defaults are generally good places to start: integer types default to `i32`.

Other types of numbers can also be represented like these:

| Number Literals | Example |
| --- | --- |
| Decimal | `98_222` |
| Hex | `0xff` |
| Octal | `0o77` |
| Binary | `0b1111_0000` |
| Byte(`u8` only) | `b'A'` |

You can find examples of these here: [https://github.com/codeTIT4N/rust-school/blob/main/lesson-3/variables/src/main.rs](https://github.com/codeTIT4N/rust-school/blob/main/lesson-3/variables/src/main.rs)

Understanding Integer Overflow:

Let’s say you have a variable of type `u8` that can hold values between 0 and 255. If you try to change the variable to a value outside that range, such as 256, *integer overflow* will occur, which can result in one of two behaviors. Let's see those using a code example:

1. If I compile this code using `cargo run` (debug mode):
    
    ```rust
    fn main(){
        let mut overflow = 255u8;
        overflow = overflow + 1;
        println!("Value of overflow is: {overflow}");
    }
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676037557813/b338c0ba-d3ee-45f0-ba7c-467f0ce0134a.png align="center")
    
    Here, you can see it "panics". We will learn about panics in detail later in the series. But for now, think of panic as a runtime error.
    
2. But if I compile it using `cargo run --release` (release mode):
    
    You will see:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676037708534/020e9bd6-0683-468f-b2da-8ddde19d38b8.png align="center")
    
    Here, you can see it did not panic. Instead, it gave a wrong answer. This behavior in rust is called *two’s complement wrapping.*
    
    Since the range of a u8 is from 0 - 255. So, when we added 1 to 255 it wrapped it to the minimum value of 0. And if I have added 2 to the overflow variable it would have given 1.
    

Why did the Rust program not panic while compiling in release mode? This is because in release mode Rust does not include checks for integer overflow, which it does for the debug mode.

Now, there are ways to handle this wrapping behavior in Rust. But we won't be discussing them here. If you are curious you can check out: [https://doc.rust-lang.org/book/ch03-02-data-types.html#integer-overflow](https://doc.rust-lang.org/book/ch03-02-data-types.html#integer-overflow)

**Floating-Point Types**

For representing decimal numbers we use this type. Example:

```rust
let floating_num = 2.5;
```

This will have a default type of `f64`. Which is a 64-bit floating point variable.

Side Note: Floating-point numbers are represented according to the [IEEE-754](https://en.wikipedia.org/wiki/IEEE_754) standard.

**Using type annotations:**

We can also use type annotations like this:

```rust
let floating_num: f32 = 2.5;
```

This is a `f32` type variable. Note how we used the `:` and then gave the type. This is how you use type annotations in Rust.

Rust’s floating-point types are `f32` and `f64`, which are 32 bits and 64 bits in size, respectively.

**Boolean Type**

Similarly, we can have boolean types like:

```rust
let boolean_var = true;
let another_boolean_var: bool = false;
```

Booleans are one byte in size.

**The Character Type**

Rust’s `char` type is the language’s most primitive alphabetic type. Examples:

```rust
let p = 'p';
let z: char = 'z';
let emoji = '😄';
println!("Values of p, z, emoji: {p}, {z}, {emoji}");
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676040404866/85daf992-f5e6-4029-a3d3-3cac3fa16dad.png align="center")

Note that we specify `char` literals with single quotes, as opposed to string literals, which use double quotes. Rust’s `char` type is four bytes in size and represents a Unicode Scalar Value, which means it can represent a lot more than just ASCII. "Unicode Scalar Values range from `U+0000` to `U+D7FF` and `U+E000` to `U+10FFFF` inclusive. However, a “character” isn’t really a concept in Unicode, so your human intuition for what a “character” is may not match up with what a `char` is in Rust." We'll discuss this later in detail in this series.

### Numeric Operations

Rust supports the basic mathematical operations you’d expect for all the number types: addition, subtraction, multiplication, division, and remainder(modulo).

Examples:

```rust
// addition
let sum = 5 + 10;

// subtraction
let difference = 95.5 - 4.3;

// multiplication
let product = 4 * 30;

// division
let quotient = 5.0 / 3.0; // Results in 1.6666666666666667
let truncated = -5 / 3; // Results in -1

// remainder
let remainder = 43 % 5;
```

Note: Numeric operations in Rust are similar to other languages. So, for instance, if you do: `let quotient = 5 / 3;` it will result in 1 because both the numerator and denominator are integers, so the result will be an integer.

### Constants (vs Variables)

As the name suggests constants should always remain constant and variables can vary. But, wait aren't immutable variables similar to constants, since both of them cannot change?

The main difference is that the immutable variables are by default immutable and can be made mutable using `mut` . But the constants are always immutable, their behaviors cannot be changed.

NOTE: One thing to remember is that in certain programming constructs, we always want predictable behaviors from our code. It is very useful since it makes our compiler do less work. And less work means better performance.

Now, let's declare some constants:

```rust
const TWO: u32 = 2;
```

This is how we declare constants using the `const` keyword.

Note that it is mandatory to type annotate constants.

Thanks!