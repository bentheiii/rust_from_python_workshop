In functional programming, functions are first-class objects. We can do this in rust.

```rust
fn inc(x: isize)->isize{x+1}
fn dec(x: isize)->isize{x-1}
fn identity(x: isize)->isize{x}

fn move_to_zero(x: isize)->isize{
    let call = if x == 0 {identity} else if x > 0 {dec} else {inc};
    call(x)
}
```

this is called the function pointer type, an is marked as `fn(<args>) -> output`

```rust
fn apply_many_times<T>(initial: T, times: usize, func: fn(T)->T)->T{
    let mut ret = initial;
    for _ in 0..times{
        ret = func(ret);
    }
    ret
}

fn main() {
    let x = apply_many_times(-15, 30, move_to_zero);
    println!("{x:?}");  // 0
}
```

We can also use anonymous (lambda) functions instead of creating a named function. Anonymous function are created with the syntax `|<args>| {<body>}`, or `|<args>| <return value>` if no statements are needed.

```rust
fn main() {
    let func = |x: i32| x*2 - 1;
    let x = apply_many_times(3, 6, func);
    println!("{x:?}");  // 129
}
```

Just like in type interpolation, you can usually let rust figure out the type on its own.

```rust
fn main() {
    let func = |x| {
        let mut i = x;
        let mut ret = 0;
        while i != 0{
            ret += i%10;
            i /= 10;
        }
        ret
    };
    let x = apply_many_times(999_999, 3, func);
    println!("{x:?}");  // 9
}
```