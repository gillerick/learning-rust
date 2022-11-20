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


