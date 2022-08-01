Let's try to create a lambda function that uses an outside variable

```rust
let step = 3isize;
let func = |x| {x+step}; // this will fail
let x = apply_many_times(15, 3, func);
```

What happened? function pointers cannot capture outsize variables. Any anonymous function that tries to capture an outside variable results in a different type called a closure.

```
   |
26 |     let func = |x| {x+step}; // this will fail
   |                ------------ the found closure
27 |     let x = apply_many_times(15, 3, func);
   |                                     ^^^^ expected fn pointer, found closure
   |
   = note: expected fn pointer `fn({integer}) -> {integer}`
                 found closure `[closure@src/main.rs:26:16: 26:28]`
```

While we can return closures, we can't actually specify their types directly. We need to use a trait that all callable types share: `Fn`.

```rust
fn apply_many_times<T, F>(initial: T, times: usize, func: F)->T
where F: Fn(T)->T
{
    let mut ret = initial;
    for _ in 0..times{
        ret = func(ret);
    }
    ret
}
```

Note that each closure has its own type, they cannot be used together

```rust
let step = 3isize;

let func = if rand::random() {|x| x+step} else {|x| x*step};  // this will fail

let x = apply_many_times(15, 3, func);
```

# Exercise
Make the above program work: we want a program which will 50% of the time output 24 (`15+3+3+3`) and 50% will output 405 (`15*3*3*3`)