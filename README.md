## Learning Rust

This project is for learning fundamentals of Rust programming language

### Primitives

#### Scalar Types

- These include:
    - _signed integers_
    - _unsigned integers_
    - _floating point_
    - _bool_
    - the unit type (), whose only possible value is an empty tuple: ()

#### Compound Types

- These include:
    - arrays like `[1, 2, 3]`
    - tuples like `(1, "two", false)`

- Variables can always be type annotated. Numbers may additionally be annotated via a suffix or by default. Integers
  default to i32 and floats to f64.
- Rust can also infer types from context.

### Literals and Operators

- Integers 1, floats 1.2, characters 'a', strings "abc", booleans true and the unit type () can be expressed using
  literals.
- Integers can, alternatively, be expressed using hexadecimal, octal or binary notation using these prefixes
  respectively:
  `0x`, `0o` or `0b`.
- Underscores can be inserted in numeric literals to improve readability,
- e.g. 1_000 is the same as 1000, and 0.000_001 is the same as 0.000001.
- We need to tell the compiler the type of the literals we use. For now, we'll use the u32 suffix to indicate that the
  literal is an unsigned 32-bit integer, and the i32 suffix to indicate that it's a signed 32-bit integer.

### Tuples

- A tuple is a collection of _values of different types_.
- Tuples are constructed using parentheses (), and each tuple itself is a value with type signature (T1, T2, ...), where
  T1, T2 are the types of its members.
- Functions can use tuples to return multiple values, as tuples can hold any number of values.
