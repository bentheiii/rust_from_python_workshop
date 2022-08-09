in order to ensure synchronization, rust uses a simple rule when using references:

At any time, a value can either be used mutably once, or borrowed immutably many times, but never both.

```rust
let mut x = 0;
let s = &x;
let m = &mut x;
*m += 1;
println!("{s:?}");  // this will fail
```

```
error[E0502]: cannot borrow `x` as mutable because it is also borrowed as immutable
 --> src/main.rs:4:9
  |
3 | let s = &x;
  |         -- immutable borrow occurs here
4 | let m = &mut x;
  |         ^^^^^^ mutable borrow occurs here
5 | *m += 1;
6 | println!("{s:?}");  // this will fail
  |            - immutable borrow later used here
```