Now we'll introduce something we've seen before: generic specialization.

Consider the function:

```rust
fn default_plus_two<T>()->T
    where T: Default + From<i32> + Add<T, Output=T>
{
    return T::default() + T::from(2)
}
```

And the usage:

```rust
let x = default_plus_two();
println!("{x}");
```

This will fail, since rust can't tell what type `x` should be. In cases like this we have to explicitly tell it with the turbofish (`::<>`) operator.

```rust
let x = default_plus_two::<f64>();
println!("{x}");  // 2.0
```

In many cases rust supports type interpolation, so we can tell it what type it should be, and rust will figure it out for us.

```rust
let x: f64 = default_plus_two();
println!("{x}");  // 2.0
```

Type interpolation can even be deferred to later calls, or even the return type.

```rust
fn interpolate_with_return_type()->f64{
    let x = default_plus_two();  // f64
    x
}

fn dummy_function(value: f64){...}

fn interpolate_with_call(){
    let x = default_plus_two();  // f64
    dummy_function(x);
}
```

In some cases, we don't actually want to specify the entire type even when explicit, so we can trade in the underscore type (`_`) to let rust interpolate a type for us.

```rust
let mut x = Default::default();
x.push(5.0);
println!("{x:?}");  // this won't work
```

```rust
let mut x: Vec<_> = Default::default();
x.push(5.0);
println!("{x:?}");  // [0.5]
```