---
title: "Some notes for learning Rust after learning a language: Kotlin, Golang or C++,..."
excerpt: "This post is used for testing whether posts can be available on the website. Also, I am learning Rust so I want to post this for sharing."
categories:
  - Programming Language
tags:
  - Rust
  - Memory Management
  - Borrowing and Ownership
---

### Hello, world
- println!() with ! is a macro.
- String interpolation with multiple ways {}, {0} {1}, {greeting} {name} with greeting = “abc” and name = “xyz”

### Cargo

- Built-in package manager and build system
- Create new proj, init project in directory, build, run, generate project documentation,..
- Crate: package shared on crates.io

### Comments

- Should use line comment
- Rustdoc
    - Doc comments (can use Markdown inside comments)
    - Doc attributes

### Variables (Rust is a statically typed language)

- All of variables are immutable ⇒ using `mut` keyword.
- Constants and statics → annotate the type
- Variable Shadowing (same variables but different data type)

### Functions
```
//With closures
fn main() {
    let x = 2;
    let square = |i: i32| -> i32 { // Input parameters are passed inside | | and expression body is wrapped within { }
        i * i
    };
    println!("{}", square(x));
}

//Without type declarations of input and return types
fn main() {
    let x = 2;
    let square = |i| i * i; // { } are optional for single-lined closures
    println!("{}", square(x));
}

```

### Special data types
- f32, f64 (float)
- u32, u64 (uint)
- Tuple
- Slice

### Loop
- Loop, and loop with label

### Enum
Enum can have: unit variant, struct variant and tuple variant

```
enum FlashMessage{
  Success, // A unit variant
  Warning{ category: i32, message: String }, // A struct variant
  Error(String)
}
```

### Struct
- Tuple structs

```
struct Color(u8, u8, u8);
struct Kilometers(i32);
```

- Unit structs

```
struct Electron;

fn main() {
  let x = Electron;
}
```

### Impls and Traits
- Look like interface and class

### Ownership
A piece of data can only have one owner at a time. When a binding goes out of scope, Rust will free the bound resources. This is how Rust achieves memory safety

### Borrowing
- Two types of borrowing:
    - Shared borrowing
    - Mutable borrowing
- Rules of borrowing
    1. One piece of data can be borrowed **either** as a shared borrow **or** as a mutable borrow **at a given time. But not both at the same time**.
    2. Borrowing **applies for both copy types and move types**.
    3. The concept of **Liveness** ↴
- How to use Borrowing: It’s also similar to pass-by-value and pass-by-reference

### Shared Borrowing
```
fn main() {
    let a = [1, 2, 3];
    let b = &a;
    println!("{:?} {}", a, b[0]); // [1, 2, 3] 1
}


fn main() {
    let a = vec![1, 2, 3];
    let b = get_first_element(&a);

    println!("{:?} {}", a, b); // [1, 2, 3] 1
}

fn get_first_element(a: &Vec<i32>) -> i32 {
    a[0]
}
```

### Mutable Borrowing
```
fn main() {
    let mut a = [1, 2, 3];
    let b = &mut a;
    b[0] = 4;
    println!("{:?}", b); // [4, 2, 3]
}


fn main() {
    let mut a = [1, 2, 3];
    {
        let b = &mut a;
        b[0] = 4;
    }

    println!("{:?}", a); // [4, 2, 3]
}


fn main() {
    let mut a = vec![1, 2, 3];
    let b = change_and_get_first_element(&mut a);

    println!("{:?} {}", a, b); // [4, 2, 3] 4
}

fn change_and_get_first_element(a: &mut Vec<i32>) -> i32 {
    a[0] = 4;
    a[0]
}
```

### Memory Management
- A resource can only have **one owner** at a time. When it goes **out of the scope**, Rust removes it from the Memory.
- When we want to reuse the same resource, we are **referencing** it/ **borrowing** its content.
- When dealing with **references**, we have to specify **lifetime annotations** to provide instructions for the **compiler** to set **how long** those referenced resources **should be alive**.
- But because of lifetime annotations make the **code more verbose**, in order to make **common patterns** more ergonomic, Rust allows lifetimes to be **elided/omitted** in `fn` definitions. In this case, the compiler assigns lifetime annotations **implicitly**.