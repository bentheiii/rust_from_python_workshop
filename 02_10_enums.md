* An enum, AKA "tagged union", is a type that can hold one of multiple values
  ```rust
  enum Color{
    RGB(u8, u8, u8),
    Named(String),
  }
  ```
* Each "option" of an enum is called a variant.
* Just like structs, variants can have named fields, or even no fields at all!
  ```rust
  enum Animal{
    Dog {name:String, tail_color: Color},
    Parrot {name: String},
    Fish
  }
  ```
* We can refer to a variant of an enum (and create an enum) with the access `::` operator.
  ```rust
  let pluto = Animal::Dog {name: "pluto".to_string(), tail_color: Color::RGB(251, 167, 77)};
  ```