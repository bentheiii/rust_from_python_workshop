Let's try to create a function that returns a closure

```rust
fn increment_by(rhs: isize)->impl Fn(isize)->isize{
    |x| {x+rhs}  // this will fail
}
```

output:
```
error[E0373]: closure may outlive the current function, but it borrows `rhs`, which is owned by the current function
 --> src/lib.rs:8:5
  |
8 |     |x| {x+rhs}  // this will fail
  |     ^^^    --- `rhs` is borrowed here
  |     |
  |     may outlive borrowed value `rhs`
  |
```

The problem here is that by the time our returned closure will be called, the variable `rhs` will be deleted, making it inaccessible.

What we need to do is to tell rust that the closure should take **ownership** of the variable `rhs`. We do this with the `move` keyword.

```rust
fn increment_by(rhs: isize)->impl Fn(isize)->isize{
    move |x| {x+rhs}
}
```

Note that after a `move` closure has captured a variable that does not implement `Copy`, we cannot use the variable outside of it, it has been moved into the closure.

```rust
let items = vec![1,2,1,3,7];
let f = move |a| items.len() * a;
println!("{}", f(0));
println!("{items:?}"); // this will fail
```

```
error[E0382]: borrow of moved value: `items`
  --> src/main.rs:15:16
   |
12 |     let items = vec![1,2,1,3,7];
   |         ----- move occurs because `items` has type `Vec<i32>`, which does not implement the `Copy` trait
13 |     let f = move |a| items.len() * a;
   |             -------- ----- variable moved due to use in closure
   |             |
   |             value moved into closure here
14 |     println!("{}", f(0));
15 |     println!("{items:?}");
   |                ^^^^^ value borrowed here after move
   |
```