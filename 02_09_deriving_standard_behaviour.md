* By default, many behaviors in structs are disabled. Like Equating, Printing, and Copying.
* We can recreate the "expected" behavior by using the derive macro
  ```
  #[derive(PartialEq, Eq, Debug, Clone)]
  struct A{
    ...
  }
  ```