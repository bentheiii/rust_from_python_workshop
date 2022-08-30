Macros can be variadic and accept a list of metavariables

```rust
macro_rules! tuple_of_fields {
    ($field: ident; $($ns: expr),*) => {{
        ($(
            $ns.$field
        ),*)
    }}
}

struct A{height: usize}
struct B{height: usize, bar: f64}
struct C{height: usize, foo: String}

fn main(){
    let a = A{height: 12};
    let b = B{height: 3, bar: 2.6};
    let c = C{height: 8, foo: "".to_string()};
    
    let s = tuple_of_fields!(height; a, b, c);
    println!("{s:?}") // (12, 3, 8)
}
```

Macros can also match multiple patterns

```rust
macro_rules! pow{
    ($a: expr, $b: expr) => {$a.pow($b)};
    ($a: expr, $b: expr, $c: expr) => {$a.pow($b)  % $c};
}

fn main(){
    println!("{}", pow!(2i32,3)); // 8
    println!("{}", pow!(6i32,9, 17)); // 11
}
```

Macros can even recourse
```rust
macro_rules! pow{
    ($a: expr, $b: expr) => {$a.pow($b)};
    ($a: expr, $b: expr, $c: expr) => {pow!($a, $b)};
}
```

Finally, we can recourse variadic macros to make some powerful patterns
```rust
macro_rules! sum_of_fields {
    ($field: ident; $ns: expr) => {{
        $ns.$field
    }};
    ($field: ident; $first_ns: expr, $($ns: expr),+) => {{
        $first_ns.$field + sum_of_fields!($field; $($ns),+)
    }};
}

struct A{height: usize}
struct B{height: usize, bar: f64}
struct C{height: usize, foo: String}

fn main(){
    let a = A{height: 12};
    let b = B{height: 3, bar: 2.6};
    let c = C{height: 8, foo: "".to_string()};
    
    let s = sum_of_fields!(height; a, b, c);
    println!("{s}") // 23
}
```

# Exercise 1
`then_some` is a bool function that, if true, will return an `Option` with a value filled in by a function
```rust
assert_eq!(false.then_some(0), None);
assert_eq!(true.then_some(0), Some(0));
```

implement a macro, that, given a sequence of expressions returning an `Option`, with the last one returning a value, will return the first non-`None` result, without evaluating the other expressions

```rust
first_some!(
    {println!("A"); (0==1).then_some(0)}, 
    {println!("B"); (0==1).then_some(1)},
    {println!("C"); (1==1).then_some(2)},
    {println!("D"); (0==1).then_some(3)},
    {println!("E"); (1==1).then_some(4)},
    {println!("F"); 5},
)
// shout return 2, and print A, B, and C
```