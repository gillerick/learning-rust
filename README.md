## Learning Rust

This project is for learning fundamentals of Rust programming language

### 1. Primitives

#### Scalar Types

- These include:
    - signed integers like `-2i32`
    - unsigned integers like `1u32`
    - floating point like `1.0`
    - bool like `true`
    - the unit type (), whose only possible value is an empty tuple: ()

#### Compound Types

- These include:
    - arrays like `[1, 2, 3]`
    - tuples like `(1, "two", false)`

- Variables can always be type annotated. Numbers may additionally be annotated via a suffix or by default. Integers
  default to i32 and floats to f64.
- Rust can also infer types from context.

#### 1.1. Literals and Operators

- Integers 1, floats 1.2, characters 'a', strings "abc", booleans true and the unit type () can be expressed using
  literals.
- Integers can, alternatively, be expressed using hexadecimal, octal or binary notation using these prefixes
  respectively:
  `0x`, `0o` or `0b`.
- Underscores can be inserted in numeric literals to improve readability,
- e.g. 1_000 is the same as 1000, and 0.000_001 is the same as 0.000001.
- We need to tell the compiler the type of the literals we use. For now, we'll use the u32 suffix to indicate that the
  literal is an unsigned 32-bit integer, and the i32 suffix to indicate that it's a signed 32-bit integer.

#### 1.2. Tuples

- A tuple is a collection of _values of different types_.
- Tuples are constructed using parentheses (), and each tuple itself is a value with type signature (T1, T2, ...), where
  T1, T2 are the types of its members.
- Functions can use tuples to return multiple values, as tuples can hold any number of values.

#### 1.3. Arrays and Slices

- An array is a collection of objects of the same type T, stored in contiguous memory.
- Arrays are created using brackets `[]`, and their length, which is known at compile time, is part of their type
  signature [T; length].
- Slices are similar to arrays, but their length is not known at compile time.
- Instead, a slice is a two-word object, the first word is a pointer to the data, and the second word is the length of
  the slice. The word size is the same as usize,
  determined by the processor architecture e.g. 64 bits on an x86-64. Slices can be used to borrow a section of an
  array, and have the type signature `&[T]`.

### 2. Custom types

- Rust custom data types are formed mainly through the two keywords:
    - `struct`: defines a structure
    - `enum`: defines an enumeration
- Constants can also be created via the `const` and `static` keywords.

#### 2.1. Structures

- There are three types of structures ("structs") that can be created using the struct keyword:
    - Tuple structs, which are, basically, named tuples.
    - The classic C structs
    - Unit structs, which are field-less, are useful for generics.

#### 2.2. Enums

- The `enum` keyword allows the creation of a type which may be one of a few different variants.
- Any variant which is valid as a struct is also valid as an enum.
- If you use a type alias, you can refer to each enum variant via its alias. This might be useful if the enum's name is
  too long or too generic, and you want to rename it. An example is shown below:

  ```rust
  enum VeryVerboseEnumOfThingsToDoWithNumbers {
      Add,
      Subtract,
  }
  
  // Creates a type alias
  type Operations = VeryVerboseEnumOfThingsToDoWithNumbers;
  
  fn main() {
      // We can refer to each variant via its alias, not its long and inconvenient
      // name.
      let x = Operations::Add;
  }
  ```

- A common use case for this is in `impl` blocks using the self alias

  ```rust
  enum VeryVerboseEnumOfThingsToDoWithNumbers {
      Add,
      Subtract,
  }
  
  impl VeryVerboseEnumOfThingsToDoWithNumbers {
      fn run(&self, x: i32, y: i32) -> i32 {
          match self {
              Self::Add => x + y,
              Self::Subtract => x - y,
          }
      }
  }
  ```

- In Rust enums, `use` declaration can be used so that manual scoping isn't needed. This is shown below:

  ```rust
  enum Status {
      Valid,
      Invalid,
  }
  
  enum User {
      Admin,
      Member,
  }
  
  fn main() {
      // Explicitly `use` each name so they are available without manual scoping.
      use crate::Status::{Valid, Invalid};
      // Automatically `use` each name inside `User`.
      use crate::User::*;
  
      // Equivalent to `Status::Invalid`.
      let status = Invalid;
      // Equivalent to `User::Admin`.
      let user = Admin;
  
      match status {
          // Note the lack of scoping because of the explicit `use` above.
          Valid => println!("This is a valid status"),
          Invalid => println!("This is an invalid status"),
      }
  
      match user {
          // Note again the lack of scoping.
          Admin => println!("An admin user has unrestricted permission"),
          Member => println!("A member user has limited permissions"),
      }
  }
  ```

- Enums in Rust can also be used as C-like enums. This is shown below:

  ```rust
  // enum with implicit discriminator (starts at 0)
  enum Number {
      Zero,
      One,
      Two,
  }
  
  // enum with explicit discriminator
  enum Color {
      Red = 0xff0000,
      Green = 0x00ff00,
      Blue = 0x0000ff,
  }
  
  fn main() {
      // `enums` can be cast as integers.
      println!("zero is {}", Number::Zero as i32);
      println!("one is {}", Number::One as i32);
  
      println!("roses are #{:06x}", Color::Red as i32);
      println!("violets are #{:06x}", Color::Blue as i32);
  }
  ```

#### 2.3. Constants

- Rust has two different types of constants which can be declared in any scope including global. Both require explicit
  type annotation:
    - const: An unchangeable value (the common case).
    - static: A possibly mutable variable with 'static lifetime. The static lifetime is inferred and does not have to be
      specified. Accessing or modifying a mutable static variable is unsafe.

```rust
// Globals are declared outside all other scopes.
static LANGUAGE: &str = "Rust";
const THRESHOLD: i32 = 10;

fn is_big(n: i32) -> bool {
    // Access constant in some function
    n > THRESHOLD
}

fn main() {
    let n = 16;

    // Access constant in the main thread
    println!("This is {}", LANGUAGE);
    println!("The threshold is {}", THRESHOLD);
    println!("{} is {}", n, if is_big(n) { "big" } else { "small" });

    // Error! Cannot modify a `const`.
    // THRESHOLD = 5;
    // FIXME ^ Comment out this line
}
```

### 3. Variable bindings

- Rust provides type safety via `static typing`.
- Variable bindings can be type annotated when declared. However, in most cases, the compiler will be able to infer the
  type of the variable from the context, heavily reducing the annotation burden.
- Values (like literals) can be bound to variables, using the let binding.

#### 3.1. Patterns

- In many languages, a variable binding would be called a variable, but Rust’s variable bindings have a few tricks up
  their sleeves. For example the left-hand side of a let statement is a ‘pattern’, not a variable name. This means we
  can do things like:

  ```rust
  fn main() {
      let (x, y) = (1, 2);
  }
  ```

- After this statement is evaluated, x will be one, and y will be two.

#### 3.2. Type annotations

- Rust is a statically typed language, which means that we specify our types up front, and they’re checked at compile
  time. So why does our first example compile? Well, Rust has this thing called ‘type inference’. If it can figure out
  what the type of something is, Rust doesn’t require you to explicitly type it out.
- We can add the type if we want to, though. Types come after a colon (:):

  ```rust
  fn main() {
      let x: i32 = 3;
  }
  ```

- This can be read out loudly as “x is a binding with the type i32 and the value 3.”
- In this case we chose to represent x as a 32-bit signed integer. Rust has various primitive integer types. They
  begin with i for signed integers and u for unsigned integers.
- The possible integer sizes are 8, 16, 32, and 64 bits.

#### 3.3. Mutability

- Variable bindings are immutable, though this can be overridden using the `mut` modifier as shown below:

  ```rust
  fn main() {
      let _immutable_binding = 1;
      let mut mutable_binding = 1;
  
      println!("Before mutation: {}", mutable_binding); //prints out 1
  
      mutable_binding += 2;
  
      println!("After mutation: {}", mutable_binding); // prints out 3
  
      // Fails with error `cannot assign twice to immutable variable `_immutable_binding`
      _immutable_binding += 1;
  }
  ```



