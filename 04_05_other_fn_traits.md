In truth, `Fn` is the most powerful callable trait, and it is also the hardest to implement.

```rust
fn increment_by<T: Add<I>, I>(rhs: I)->impl Fn(T)->T::Output{
    move |x| {x+rhs}  // this will fail
}
```

```
error[E0507]: cannot move out of `rhs`, a captured variable in an `Fn` closure
 --> src/main.rs:4:17
  |
3 | fn increment_by<T: Add<I>, I>(rhs: I)->impl Fn(T)->T::Output{
  |                               --- captured outer variable
4 |     move |x| {x+rhs}
  |     ------------^^^-
  |     |           |
  |     |           move occurs because `rhs` has type `I`, which does not implement the `Copy` trait
  |     captured by this `Fn` closure
```

This is because rust cannot guarantee that the returned function can be used twice, because it must lose ownership of the moved `rhs` when adding it into `x`. We can solve this by returning a weaker type than `Fn`, we can return `FnOnce`

```rust
fn increment_by<T: Add<I>, I>(rhs: I)->impl FnOnce(T)->T::Output{
move |x| {x+rhs}
}
```

`FnOnce` is a closure that can only be safely called once.

```rust
fn call_twice(f: impl FnOnce()->()){
    f();
    f();  // this will fail
}
```

```
error[E0382]: use of moved value: `f`
  --> src/main.rs:13:5
   |
11 | fn call_twice(f: impl FnOnce()->()){
   |               - move occurs because `f` has type `impl FnOnce() -> ()`, which does not implement the `Copy` trait
12 |     f();
   |     --- `f` moved due to this call
13 |     f();
   |     ^ value used here after move
```

We can also use `FnMut` for a callable that mutates its own state.

```rust
fn fib()->impl FnMut()->usize{
    let mut a = 0;
    let mut b = 1;
    
    move || {
        let ret = b;
        b += a;
        a = ret;
        ret
    }
}

fn main(){
    let mut f = fib();
    let mut v = vec![];
    for _ in 0..10{
        v.push(f());
    }
    println!("{v:?}");  // [1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
}
```

Remember that both `Fn` subtraits `FnMut`, and that `FnMut` subtraits `FnOnce`

# Exercise 1

Implement a partial function, that accepts a callable that accepts two arguments, and the first argument, and returns a function that returns accepts the second argument.

```rust
let original_func = |a,b| a+b*2;
let partial_func = partial(original_func, 5); // you need to implement this
assert_eq!(partial_func(partial_func(10), 5+10*2))
```

# Exercise 2 (advanced)

Create a "memoized" wrapper around a function, that if the same argument is given to it twice, it will only calculate once.