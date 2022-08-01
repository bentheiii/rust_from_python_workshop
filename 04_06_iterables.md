Lets talk about iterables.

Iterables are an abstraction over ability to iterate over a collection. The upside is that **lazy** iterators don't need to generate all their members at once, and only require a limited amount of space.

```rust
let my_vec = vec![1,2,3,5,8,13,21,34,55,89];
// if we want to get the square of the first even number higher than 20, we can do:
let iterable = my_vec.iter();
let filtered = iterable.filter(|&x| x > &20 && x % 2 == 0);
let mapped = filtered.map(|&x| x*x);
let first = mapped.nth(0);
// or in one line
let first_one_line = my_vec.iter().filter(|&x| x > &20 && x % 2 == 0).map(|x| x*x).nth(0);
println!("{:?}", first_one_line); // Some(1156)
```

Rust has very powerful iterable handling in its standard library