# References and Borrowing in Rust

## GitHub repo with all the code

[https://github.com/codeTIT4N/rust-school](https://github.com/codeTIT4N/rust-school)

For this lesson: [https://github.com/codeTIT4N/rust-school/tree/main/lesson-7](https://github.com/codeTIT4N/rust-school/tree/main/lesson-7)

Make sure to star/fork/watch it on GitHub.

## Prerequisites

To understand this article you must be familiar with the concept of ownership in Rust. For that refer to my [previous article](https://blog.lokeshkr.com/rust-series-6).

## References

Consider the code:

```rust
fn main() {
    let s1 = String::from("hello");

    let (s2, len) = calculate_length(s1);

    println!("The length of '{}' is {}.", s2, len);
}

fn calculate_length(s: String) -> (String, usize) {
    let length = s.len();

    (s, length)
}
```

Here, we are returning the `String` after using it in the `calculate_length` . But this is not convenient. To solve this issue, we have references in Rust.

### What is a Reference?

A *reference* is like a pointer in that it’s an address we can follow to access the data stored at that address; that data is owned by some other variable.

### How is a reference different from a pointer?

Unlike a pointer, a reference is guaranteed to point to a valid value of a particular type for the life of that reference.

If you have worked with pointers in languages like C/C++, you know there is a problem where the pointer can point to unallocated memory and cause problems in your program. But this is not the case in Rust.

A reference in Rust will always point to a valid value. This is where Rust shines.

### Using references

The above code example can be modified like this:

```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

Notice the use of `&` operator for passing references. And we declare its type in the parameter using `&String` . Also notice that we do not need the 2nd parameter anymore. Output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677511889915/fc26ea1c-1887-4f82-9593-006e5e5fa408.png align="left")

These ampersands represent *references*, and they allow you to refer to some value without taking ownership of it.

Side Note: Rust also provides the reverse of it which is dereferencing, which we will learn about in upcoming articles.

### Under the hood

This is what is happening under the hood:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677512509003/05fa8218-e2c2-4447-9f7f-517660f76c3c.svg align="center")

Here, `s` of type `&String` is pointing at `String s1` .

The `&s1` syntax lets us create a reference that *refers* to the value of `s1` but does not own it. Because it does not own it, the value it points to will not be dropped when the reference stops being used.

So, When functions have references as parameters instead of the actual values, we won’t need to return the values in order to give back ownership, because we never had ownership.

### Borrowing

This method of creating a reference is called *borrowing.*

### Can we modify something borrowed?

The answer is no. The reason is because just like variables, references are immutable by default. So, if I try to do:

```rust
fn calculate_length(s: &String) -> usize {
    s.push_str(", world"); // will not work
    s.len()
}
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677512974068/7941fb82-09e7-40e8-a2a8-ee6d491ba850.png align="center")

You can see we got an error and as usual, the Rust compiler is kind enough to explain it very nicely.

### Mutable References

I am sure you can guess by now that by using the `mut` we can make our reference mutable, just like we did in the case of variables. These are called *mutable references.*

Here is an example:

```rust
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

Notice the use of `&mut` instead of `&` . This is how you declare mutable references. Also, notice the `&mut String` in the function arguments.

This makes it very clear that the `change` function will mutate the value it borrows.

### Mutable references and the infamous Data Races

Let's start with the basics: **What are Data Races?**

One answer I found online:

*Data races are a common problem in multithreaded programming. Data races occur when multiple tasks or threads access a shared resource without sufficient protections, leading to undefined or unpredictable behavior.*

It is similar to a race condition and happens when these three behaviors occur:

* Two or more pointers access the same data at the same time.
    
* At least one of the pointers is being used to write to the data.
    
* There’s no mechanism being used to synchronize access to the data.
    

We don't need to go deep into it here.

**The Rust solution:**

Rust has a solution for the data race problem. Which is that *if you have a mutable reference to a value, you can have no other references to that value.*

Consider the code:

```rust
let mut s = String::from("hello");

let r1 = &mut s;
let r2 = &mut s;

println!("{}, {}", r1, r2);
```

Now, in the above code, we are trying to create more than one mutable reference to the same data, which is stored in `s` . Rust will not allow this.

We get:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677514670354/ae592198-e4df-489c-9c3c-37838edda5a8.png align="center")

This error says that this code is invalid because we cannot borrow `s` as mutable more than once at a time. The first mutable borrow is in `r1` and must last until it’s used in the `println!`, but between the creation of that mutable reference and its usage, we tried to create another mutable reference in `r2` that borrows the same data as `r1`.

The benefit of having this restriction is that Rust can prevent data races at compile time.

Data races cause undefined behavior and can be difficult to diagnose and fix when you’re trying to track them down at runtime; Rust prevents this problem by refusing to compile code with data races!

### Using multiple mutable references despite the restriction

If we want we can use the concept of scope(which we learned [here](https://blog.lokeshkr.com/rust-series-4#heading-scopes)) to use multiple mutable references to the same data like:

```rust
let mut s = String::from("hello");

{
    let r1 = &mut s;
} // r1 goes out of scope here, so we can make a new reference with no problems.

let r2 = &mut s;
```

This code is pretty self-explanatory if you are aware of [scopes in Rust](https://blog.lokeshkr.com/rust-series-4#heading-scopes).

### More Rust restrictions while using References

Now, there is another restriction that rust has while using references. As we learned in the earlier part of this article you cannot have 2 mutable references pointing to a single data. Rust enforces a similar rule for combining mutable and immutable references. This code results in an error:

```rust
let mut s = String::from("hello");

let ref1 = &s; // no problem
let ref2 = &s; // no problem
let ref3 = &mut s; // PROBLEM

println!("{}, {}, and {}", ref1, ref2, ref3);
```

We get:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677515744691/b3a6d004-9d4d-48e6-a72c-9b759c74b5ea.png align="center")

Here, Rust is saying that: We *also* cannot have a mutable reference while we have an immutable one to the same value.

This is because Users of an immutable reference don’t expect the value to suddenly change out from under them! However, multiple immutable references are allowed because no one who is just reading the data has the ability to affect anyone else’s reading of the data.

### A complicated scenario to fully understand the concept

Consider the code:

```rust
let mut s = String::from("hello");

let reference1 = &s;
let reference2 = &s;
println!("{} and {}", reference1, reference2);

let reference3 = &mut s;
println!("{}", reference3);
```

We know from our previous examples that there will be no problem in `let reference1 = &s;` and `let reference2 = &s;` . But in `let reference3 = &mut s;` we are trying to create a mutable reference after already having an immutable reference. So, this should result in an error, Right?

But this does not happen. Output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677522958026/e39a78e2-8651-4c58-a0f6-5638ee96ff4c.png align="left")

Why is that?

To understand this we have to keep in mind the scope of a reference.

**A reference’s scope starts from where it is introduced and continues through the last time that reference is used.**

The above code will compile because the last usage of the immutable references, the `println!`, occurs before the mutable reference is introduced. The scopes of the immutable references `reference1` and `reference2` end after the `println!` where they are last used, which is before the mutable reference `reference3` is created. These scopes don’t overlap, so this code is allowed: the compiler can tell that the reference is no longer being used at a point before the end of the scope.

Note: Even though borrowing errors may be frustrating at times, remember that it’s the Rust compiler pointing out a potential bug early (at compile time rather than at runtime) and showing you exactly where the problem is. Then you don’t have to track down why your data isn’t what you thought it was.

### Dangling References

This concept is a little bit difficult to explain. So, let me take an example:

```rust
fn main() {
    let reference_to_nothing = dangle();
}

fn dangle() -> &String {
    let s = String::from("hello");

    &s
}
```

So, what is happening here? When we call the `dangle` function it will return a reference to a string. But the problem is that since the variable `s` will be dropped after the execution of this function, the `dangle` function will return a reference to something which do not exist.

This is a common problem in many languages. But this code will not compile in Rust. So, the Rust compiler can protect us from dangling references also.

We get:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677524225247/4de1d69c-81f5-458d-b5f2-cdea9e0fd1e5.png align="center")

Here, it is talking about lifetimes which is another Rust concept that we will look into in upcoming articles. But if you ignore that part in the `help` it explains the problem very well.

**Solution**:

To fix this we can just not use `borrowing` in this code and opt for the normal `move` approach. Consider:

```rust
fn no_dangle() -> String {
    let s = String::from("hello");

    s
}
```

This works without any problems. Ownership is moved out, and nothing is deallocated/dropped.

## Recap

Keep these in mind while working with References:

* At any given time, you can have *either* one mutable reference *or* any number of immutable references.
    
* References must always be valid.
    

## A Suggestion

I know things are getting a bit complicated. So, let me share with you what I do to understand things and retain them.

I start coding. You will not make a strong Rust foundation until you code with it, and experiment with it. One way to do that is to use something like [Rustlings](https://github.com/rust-lang/rustlings).

Which is a very good tool to get used to reading and writing Rust code. Just clone the repo and follow the instructions and start coding. You will learn a lot. And if you get stuck you can always come back to this article(or any article of this series for that matter).

Thank you!