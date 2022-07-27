variables in rust
* variables are created with the `let` statement
  `let <name>: <type> = <value>;`
* for example: `let x: i32 = 500;`
* The type can be omitted in many cases: `let x = vec![16, 17];`
* Sometimes the type must be explicit: `let x: Vec<String> = vec![];`
* We can even omit the initial value if we provably set the value later
  ```rust
  let x;
  if rand::random() {
      x = -1;
  } else {
      x = 1;
  }
  ```
* By default, variables in rust are immutable. This means that they cannot change once assigned. Since rust is more functional than other low level languages, this usually results in cleaner code, but mutable variables can be created with the `mut` keyword.
  ```rust
  let mut x = 1;
  for i in 1..=10 {
      x += i;
  }
  assert_eq!(x, 55);
  ```
* again, since rust is a functional language, this can be made more readable with an immutable `x`
  ```rust
  let x: i32 = (1..=10).sum();
  assert_eq!(x, 55);
  ```