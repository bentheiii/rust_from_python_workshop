```rust
macro_rules! first_some {
    ($t: expr) => {{
        $t
    }};
    ($first: expr, $($parts: expr),+ $(,)?) => {{
        $first.unwrap_or_else(|| first_some!($($parts),+))
    }};
}

fn main() {
    let s = first_some!(
        {println!("A"); (0==1).then_some(0)}, 
        {println!("B"); (0==1).then_some(1)},
        {println!("C"); (1==1).then_some(2)},
        {println!("D"); (0==1).then_some(3)},
        {println!("E"); (1==1).then_some(4)},
        {println!("F"); 5},
    );
    println!("{s}")
}
```