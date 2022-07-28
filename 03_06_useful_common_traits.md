There are a lot of useful traits in the standard library that you can use. Some of them can be can be `derive`d in structs and enums (so long as its elements also implement the trait)
* `Clone`: allows to copy a value explicitly.
* `Copy`: a "marker trait" that marks a type as allowing to `clone` an object implicitly.
  * Rule of thumb: only small object should implement `Copy`
* `Drop` to implement destructors
* `PartialEq`/`PartialOrd` to implement comparisons.
  * Note that `PartialEq` and `PartialOrd` are generic, so for you can compare two unrelated types (for example, `&[usize]` implements `PartialEq<Vec<usize>>`)
* `Eq`/`Ord` to implement absolute comparisons.
* `Hash` to allow for the type to be used as a key in a hashmap.
* `Default` to allow creating default elements of a type.
* `Display` and `Debug`, two traits which allow for converting to string, and allows for printing macros to work.
* `Into`/`From` to allow conversion between types. Prefer implementing `From`.
* `Add`/`Sub`/`Mul`/... allows for operator overloading
* `IntoIterator` and `Iterator`, which we will cover later.

# Exercise
given the following struct
```rust
struct Complex<T>{real: T, imaginary: T}; // complex number
```
implement the following traits (do not use `derive`):

* equality
* cloning
* Addition/Subtraction with other complexes
* Multiplication/Division with scalars
* `Debug`
* order: a complex number is "larger" than another complex number if they have the same imaginary part, and its real part is higher than the other's.
* hashing, Advanced
* `From` from a real part (for example, 5 when converted to a complex should be `Complex{real: 5, imaginary: 0}`), Advanced