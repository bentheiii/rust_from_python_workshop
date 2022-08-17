A rather new addition to Rust is the addition of const generics, making a type changeable by an integral constant

```rust
struct Pixel<D, const M: usize> {
    coordinates: [f64; M],
    data: D
}
```

Note that this mechanic is extremely limited
```rust 
struct Islands<const M: usize> {
    islands: [Island; M],
    bridges: [Bridge; M-1] // this won't work
}
```

One possible use is feature-flags at compile time
```rust
fn foo<const AdvancedLogs: bool>(...){
    ...
    if AdvancedLogs{  // checked at compile-time, not runtime
        ...
    }
    ...
}

let callable_without_logs = foo::<false>;
let callable_with_logs = foo::<true>;
```