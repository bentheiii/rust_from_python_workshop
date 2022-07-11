in order to ensure synchronization, rust uses a simple rule when using references:

At any time, a value can either be used mutably once, or borrowed immutably many times, but never both.

```rust
let mut x = 0;
let s = &x;
x += 1;
println!("{s:?}");
```