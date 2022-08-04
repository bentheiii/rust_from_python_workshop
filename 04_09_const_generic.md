A rather new addition to Rust is the addition of const generics, making a type changeable by an integral constant

```rust
struct Point<const D: usize>([])
```