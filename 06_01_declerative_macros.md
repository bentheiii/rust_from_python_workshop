A very powerful object in rust is the declarative macro. They allow us to make function-like item that creates code.

```rust
macro_rules! get_field{
    ($field: ident, $item0: expr) => {$item0.$field}
}

struct Foo{x: i32, y:i32}
fn bar(f: Foo)->i32{
    get_field!(y, f) // will create the syntax: f.y
}
```

we can use this mechanism to do things that would be difficult without

```rust
struct Person{height: usize, name: String}
struct Ape{height: usize}

macro_rules! add_fields{
    ($field: ident, $item0: expr, $item1: expr) => {$item0.$field + $item1.$field}
}

fn bar(person: Person, ape: Ape)->usize{
    add_fields!(height, person, ape)
}
```