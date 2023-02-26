# Rust concepts: Compound Types, Scopes, Shadowing, and Functions

## GitHub repo with all the code

[https://github.com/codeTIT4N/rust-school](https://github.com/codeTIT4N/rust-school)

For this lesson: [https://github.com/codeTIT4N/rust-school/tree/main/lesson-4](https://github.com/codeTIT4N/rust-school/tree/main/lesson-4)

Make sure to star/fork/watch it on GitHub.

## Compound Types

In the [last blog of the series](https://blog.lokeshkr.com/rust-series-3#heading-revisiting-data-types-and-using-type-annotations), we learned about Data Types, which can be broadly classified into Scalar Types([already coved](https://blog.lokeshkr.com/rust-series-3#heading-revisiting-data-types-and-using-type-annotations)) and Compound Types. Let's talk about Compound Types here.

*Compound types* can group multiple values into one type. Rust has two primitive compound types: tuples and arrays.

### The Tuple Type

Consider this example:

```rust
let tup = (450, 6.2, 1);
println!("{:?}", tup);
```

This is how you define a tuple. Each position in the tuple has a type, and the types of the different values in the tuple don’t have to be the same.

NOTE: To display a tuple we are using the "{:?}" token. We will learn about this in detail in future lessons.

If you want you can also explicitly type annotate this like:

```rust
let tup: (i32, f64, u8) = (450, 6.2, 1);
```

The variable `tup` binds to the entire tuple because a tuple is considered a single compound element.

To get the individual values out of a tuple:

```rust
let (x, y, z) = tup;

println!("The value of x is: {x}");
println!("The value of y is: {y}");
println!("The value of z is: {z}");
```

This will retrieve each value from the tuple in x,y, and z and print it. Output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676297420800/be2addcb-7ce2-4498-9d82-c1a8171ade29.png align="left")

We are using pattern matching to destructure a tuple value here.

**Another way to access tuple elements:**

Consider the code:

```rust
let tuple: (i32, f64, u8) = (500, 6.4, 1);

let a = tuple.0; // accessing a tuple element
let b = tuple.1;
let c = tuple.2;

println!("The value of a is: {a}");
println!("The value of b is: {b}");
println!("The value of c is: {c}");
```

Here, we are using the period(`.`) followed by the index of the value we want to access. As with most programming languages, the first index in a tuple is 0.

Output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676298417524/ea28ce0f-ee94-4a1d-9a0b-0a216afd5bd9.png align="left")

The tuple without any values has a special name, *unit*. This value and its corresponding type are both written `()` and represent an empty value or an empty return type.

NOTE: Expressions implicitly return the unit value if they don’t return any other value.

**Important points to note about tuples:**

* You can combine different types of data to return from a function.
    
* Data is stored anonymously i.e., there is no need to name the fields of a tuple.
    
* Expressions implicitly return the unit value if they don’t return any other value.
    

### The Array Type

Arrays in Rust are similar to most languages. Unlike tuples, every element of an array must have the same type. The important thing to note is that arrays in Rust have a fixed length.

Example:

```rust
let arr = [1, 2, 3, 4, 5];
```

Arrays are useful when you want your data allocated on the stack rather than the heap (we will discuss this in the coming lessons) or when you want to ensure you always have a fixed number of elements.

NOTE: An array isn’t as flexible as the vector type, though. A *vector* is a similar collection type provided by the standard library that *is* allowed to grow or shrink in size. We'll discuss this later.

Arrays are more useful when you know the number of elements will not change. Example:

```rust
let days = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"];
```

In most cases, Rust can infer the length and type of array, but you can also annotate it like this:

```rust
let array: [i32; 5] = [1, 2, 3, 4, 5];
```

Here, the type is i32 and the size of the array is 5.

Accessing and displaying array elements:

Array elements can be accessed similarly to any other programming language using the index.

Example:

```rust
let array: [i32; 5] = [1, 2, 3, 4, 5];
println!("Type annotated array: {:?}", array);

// Accessing array elements
let new_array = [1, 2, 3, 4, 5];
let fist_element = new_array[0];
let second_element = new_array[1];

println!("Array: {:?}", new_array);
println!("fist element is: {fist_element}");
println!("second element is: {second_element}");
```

Here we are using `[]` with an index in between to access the element.

Output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676303759051/3c47545f-98b5-4a48-982e-59b62ebf53b7.png align="center")

**Q: What will happen if you try to access an index which does not exist?**

A: The code will *panic* (Runtime error in rust is called a panic).

## Scopes and Shadowing

### Scopes

Rust has this concept of scopes like many programming languages. Let's understand that with an example. Consider:

```rust
fn main() {
    println!("Hello, world!");
}
```

In this simple "Hello, world!" program you can see we have written all of our code inside `{}` . It is called a block. Now, anything inside this block can live only inside this block. That is it will not be valid outside of this block.

Consider this piece of code:

```rust
fn main() {
    {
        let x = 6;
    }
    println!("Value of x is {x}");
}
```

Here, the variable x will not be valid outside the `{}` . So, if I try to run this I will get:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676385994622/7b03ee7e-0cba-4e1c-9d8a-b3119948273d.png align="center")

The rust compiler is telling me that the variable x is not valid in the scope.

### Shadowing

Now, let's consider this:

```rust
let y = 1;
let y = y + 6;
println!("Value of y: {y}");
```

**Q: What do you think will happen? Will this code execute or not?**

A: The answer is it will run and it will give the right output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676386679216/a71b5705-be3e-4ef6-92da-571722b3b81b.png align="left")

**Q: But wait, Isn't variable** `y` **immutable? Then how is it changing value?**

A: This behavior in rust is called shadowing. Rustaceans say that the first variable is *shadowed* by the second, which means that the second variable is what the compiler will see when you use the name of the variable. In effect, the second variable overshadows the first, taking any uses of the variable name to itself until either it itself is shadowed or the scope ends.

In the above example, we are shadowing a variable by using the same variable’s name and repeating the use of the `let` keyword.  
Now, let's see an example with scopes:

```rust
let y = 1;
let y = y + 6;
println!("Value of y: {y}");

{
    let y = y * 2;
    println!("The value of y in the inner scope is: {y}");
}

println!("The value of y is: {y}");
```

Output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676387121513/ec8b7fd7-7a15-4180-9f2d-b5c09d7b587c.png align="center")

This program first binds `y` to a value of `1`. Then it creates a new variable `y` by repeating `let y =`, taking the original value and adding `6` so the value of `y` is then `7` Then, within an inner scope created with the curly brackets, the third `let` statement also shadows `y` and creates a new variable, multiplying the previous value by `2` to give `y` a value of `14`. When that scope is over, the inner shadowing ends and `y` returns to being `7`

### Shadowing vs Mutability

Shadowing is different from marking a variable as `mut` because we’ll get a compile-time error if we accidentally try to reassign to this variable without using the `let` keyword.

By using `let`, we can perform a few transformations on a value but have the variable be immutable after those transformations have been completed. This can come in very handy.

The other difference between `mut` and shadowing is that because we’re effectively creating a new variable when we use the `let` keyword again, we can change the type of the value but reuse the same name.

## Functions in Rust

We have already seen the most important function in Rust: the `main` function, which is the entry point of many programs.

Example:

```rust
fn main() {
    println!("Main function.");

    another_function();
}
// function defination
fn another_function() {
    println!("Another function.");
}
```

Output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676392939100/52848aaa-f04a-4b29-b0ca-5e16e0aa32cd.png align="left")

We define a function in Rust by entering `fn` followed by a function name and a set of parentheses. The curly brackets tell the compiler where the function body begins and ends.

In the code above we have defined our `another_function` which can be called using `another_function()` . In the example, we have defined `another_function` after the `main` but we can do that before `main` as well. Rust doesn’t care where you define your functions, only that they’re defined somewhere in a scope that can be seen by the caller.

### Function parameters

We can define functions to have *parameters*, which are special variables that are part of a function’s signature. A function having parameters can accept data. Technically, the concrete values are called *arguments*, but in casual conversation, people tend to use the words *parameter* and *argument* interchangeably for either the variables in a function’s definition or the concrete values passed in when you call a function.

Example:

```rust
fn main() {
    parametrized_function(70);
}

fn parametrized_function(x: i32) {
    println!("Value sent from main(): {x}")
}
```

Here, we are sending an integer to the function `parametrized_function` . This is how to send data to a function.

Output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676394112969/4d67602f-e098-4c76-a579-037e24529058.png align="left")

Now, functions can also accept different types of data. Example:

```rust
fn main() {
    parametrized_function2(70, 'L');
}

fn parametrized_function2(x: i32, y: char) {
    println!("Values sent from main(): {x}, {y}")
}
```

Output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676394274382/acf941a1-9902-42e3-8ef5-224771d8f35e.png align="left")

### Returning data from a function

Similar to sending data you can get data back from a function. Example:

```rust
fn main() {
    println!("add function result: {:?}", add(4, 5));
}

fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

Here, by using a `-> i32` signifies what type of data the function returns. Then we are returning the result of `a + b` .

Note: Here we are returning data without using a return statement.

Output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676395037480/83e3aaf6-41e1-4cf3-bf9c-bfb3caeeeafa.png align="left")

Now, let me show you something. Let's keep the same code and just add a semicolon `;` at the end:

```rust
fn main() {
    println!("add function result: {:?}", add(4, 5));
}
 
fn add2(a: i32, b: i32) -> i32 {
    a + b;
}
```

Note: the semicolon after `a + b`

If I run the code:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676395746274/483c9bf7-2b78-483f-af2d-a4b8ef9680d9.png align="left")

It fails. The main error message, `mismatched types`, reveals the core issue with this code. The definition of the function `add2` says that it will return an `i32`, but statements don’t evaluate to a value, which is expressed by `()`, the unit type.

But why is this, and how can it be fixed?

To understand this we have to first understand the difference between expressions and statements. Which we will do in the next lesson. So, stay tuned for that.

For now, you can fix it by either removing the semicolon at the end or by using an explicit `return` statement like this:

```rust
fn main() {
    println!("add function result: {:?}", add3(4, 5));
}

fn add3(a: i32, b: i32) -> i32 {
    return a + b;
}
```

Note: In this, we explicitly told the compiler to return the result for `a + b` by using the `return` . Output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676396653074/ddecb7b2-4351-4dfa-8a0d-208e486335fe.png align="left")

**Few things to note about functions**:

* Functions encapsulate functionality.
    
* Functions can optionally accept data or return data.
    
* Functions are utilized for code organization and it makes code easier to read.
    
* Functions can be executed by "calling" the function.