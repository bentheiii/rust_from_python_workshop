There are a lot of useful traits in the standard library that you can use. Some of them can be can be `derive`d in structs and enums (so long as its elements also implement the trait)
* `Clone`: allows to copy a value explicitly with `clone`.
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