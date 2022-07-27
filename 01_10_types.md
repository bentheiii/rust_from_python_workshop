Common data types in rust:
* signed integers:
  * i8
  * i16
  * i32 <-- default for integer literals
  * i64
  * i128 <-- much slower than other numbers, avoid if you can
* unsigned integers
  * u8 <-- default for byte literals
  * u16
  * u32
  * u64
  * u128
* sizes
  * isize
  * usize
* floating-point
  * f32
  * f64
* booleans: `true` or `false`
* characters: `char` can be used to specify a single unicode character `let z: char = 'Z';`
* tuples: are not like python tuples! they store a constant number of elements of different types: `let tup: (i32, f64, u8) = (500, 6.4, 1);`
* arrays: constant length, uniform data type: `let a: [i32; 5] = [1, 2, 3, 4, 5];`
* references, which we will cover later
* the nil type: `()` which represents nothing, all functions that return nothing actually return `()`.
  ```rust
  let x: () = println!("15");
  println!("{x:?}")
  ```
  output:
  ```
  15
  ()
  ```