Here’s a concise tutorial covering Rust’s core language elements. You can follow along by creating a new Rust project with `cargo new basics && cd basics` and editing `src/main.rs`.

## Comments

Rust supports two styles of comments:

```rust
// This is a single-line comment

/* This is a
   multi-line comment */
```

Comments are ignored by the compiler and great for documentation or notes.

---

## Variables and Mutability

By default, variables are **immutable**:

```rust
let x = 5;
x = 6; // error: cannot assign twice to immutable variable
```

To make a variable mutable, use `mut`:

```rust
let mut x = 5;
println!("x is {}", x);
x = 6;
println!("x is now {}", x);
```

Rust also allows **shadowing**—redeclaring a variable with `let`:

```rust
let x = 5;
let x = x + 1; // shadows previous x
println!("x is {}", x); // prints 6
```

---

## Data Types

Rust is **statically typed**, so every value has a type known at compile time.

### Scalar Types

* **Integers**: `i8, i16, i32, i64, i128, isize` (signed) and `u8, u16, …, usize` (unsigned)

  ```rust
  let a: i32 = 10;
  let b: u8 = 255;
  ```

* **Floating-point**: `f32` and `f64` (default)

  ```rust
  let c = 3.14; // f64 by default
  ```

* **Boolean**: `bool`

  ```rust
  let is_active: bool = true;
  ```

* **Character**: `char` (4-byte Unicode scalar)

  ```rust
  let heart_eyed_cat: char = '😻';
  ```

### Compound Types

* **Tuples** group fixed types:

  ```rust
  let tup: (i32, f64, u8) = (500, 6.4, 1);
  let (x, y, z) = tup; // destructuring
  println!("The value of y is {}", y);
  ```

* **Arrays** hold elements of the same type:

  ```rust
  let arr: [i32; 4] = [1, 2, 3, 4];
  println!("First element: {}", arr[0]);
  ```

---

## Functions

Rust functions are declared with `fn`:

```rust
fn main() {
    greet("Alice");
}

fn greet(name: &str) {
    println!("Hello, {}!", name);
}
```

* Parameters must include types.
* Return values use `-> Type` and the last expression is returned (no `;`):

  ```rust
  fn add(a: i32, b: i32) -> i32 {
      a + b
  }
  ```

---

## Control Flow

Rust has the usual control structures.

### `if` Expressions

```rust
let number = 6;

if number % 4 == 0 {
    println!("divisible by 4");
} else if number % 3 == 0 {
    println!("divisible by 3");
} else {
    println!("not divisible by 4 or 3");
}

// `if` can be in a let:
let condition = true;
let number = if condition { 5 } else { 6 };
```

### Loops

* **`loop`** repeats forever (or until `break`):

  ```rust
  let mut counter = 0;
  let result = loop {
      counter += 1;
      if counter == 10 {
          break counter * 2;
      }
  };
  println!("The result is {}", result);
  ```

* **`while`** repeats while a condition holds:

  ```rust
  let mut n = 3;
  while n != 0 {
      println!("{}", n);
      n -= 1;
  }
  ```

* **`for`** iterates over collections:

  ```rust
  let a = [10, 20, 30, 40, 50];
  for element in a.iter() {
      println!("the value is: {}", element);
  }

  // with ranges:
  for number in (1..4).rev() {
      println!("{}!", number);
  }
  ```

---

## Ownership, Borrowing, and References

Rust’s unique feature is the **ownership** system, ensuring memory safety without a garbage collector.

* **Ownership rules**:

  1. Each value has an owner.
  2. Only one owner at a time.
  3. When the owner goes out of scope, the value is dropped.

```rust
let s1 = String::from("hello");
let s2 = s1; // s1 is moved into s2; s1 is no longer valid
```

To **borrow** without taking ownership, use **references**:

```rust
fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(&s1); // pass a reference
    println!("Length of '{}' is {}", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
} // reference goes out of scope, but owner remains
```

Mutable references allow modifying the borrowed value:

```rust
fn main() {
    let mut s = String::from("hello");
    change(&mut s);
    println!("{}", s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

---

## Constants and Static Variables

* **Constants**: always immutable, must annotate type, can be declared in any scope.

  ```rust
  const MAX_POINTS: u32 = 100_000;
  ```

* **Static** variables have a fixed address in memory; require `unsafe` for mutable statics.

---

## Modules and Crates

* A **crate** is a Rust compilation unit (binary or library).

* A **module** organizes code within a crate, declared with `mod` and linked via file structure:

  ```text
  src/
  ├── main.rs  // declares `mod utils;`
  └── utils.rs // defines `pub fn helper() { … }`
  ```

  In `main.rs`:

  ```rust
  mod utils;

  fn main() {
      utils::helper();
  }
  ```

* `pub` makes functions/types visible outside their module.

---

## Putting It All Together

```rust
// src/main.rs
mod utils;

const MAX_ATTEMPTS: u8 = 3;

fn main() {
    println!("Welcome to Rust Basics!");
    for _ in 0..MAX_ATTEMPTS {
        let number = utils::get_random_number();
        if number % 2 == 0 {
            println!("{} is even", number);
        } else {
            println!("{} is odd", number);
        }
    }
}

// src/utils.rs
use rand::Rng; // add rand = "0.8" to Cargo.toml

pub fn get_random_number() -> u8 {
    let mut rng = rand::thread_rng();
    rng.gen_range(1..=100)
}
```

* Demonstrates modules, `const`, loops, and external crates.

---

**Next steps:**

* Experiment by modifying examples.
* Dive deeper into Rust’s **ownership** chapter in “The Rust Programming Language” book.
* Explore **traits**, **generics**, and **error handling** (`Result`/`Option`).

Happy coding!
