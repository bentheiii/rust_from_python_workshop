functions can also be generic over lifetimes

```rust
fn longest_slice<'a, T>(slice0: &'a [T], slice1: &'a [T]) -> &'a [T]{
    if slice0.len() >= slice1.len(){
        slice0
    } else {
        slice1
    }
}
```

now we can call it and get a reference, knowing the lifetime of the reference

```rust
let v0 = vec![1,2,3,4];
let v1 = vec![9,8,7,6,5,4,3,2,1];
let longest = longest_slice(&v0, &v1);

println!("{longest:?}");  // [9, 8, 7, 6, 5, 4, 3, 2, 1]
```

note that lifetimes are not strongly typed, they can be down-casted to smaller lifetimes implicitly

```rust
let v0 = vec![1,2,3,4];
let longest = {
    let v1 = vec![9,8,7,6,5,4,3,2,1];
    longest_slice(&v0, &v1)
};

println!("{longest:?}");  // this will fail
```

output
```
error[E0597]: `v1` does not live long enough
  --> src/main.rs:14:28
   |
12 |     let longest = {
   |         ------- borrow later stored here
13 |         let v1 = vec![9,8,7,6,5,4,3,2,1];
14 |         longest_slice(&v0, &v1)
   |                            ^^^ borrowed value does not live long enough
15 |     };
   |     - `v1` dropped here while still borrowed
```

