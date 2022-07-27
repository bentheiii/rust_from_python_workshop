* Slices are continuous lengths of data.
* Since their size is not known at compile time, they can only be referenced.
```rust
let v: Vec<i32> = vec![1,2,3,4,5,6,7];
let s: &[i32] = &v[1..v.len()-1];
println!("{s:?}");  // [2,3,4,5,6]
```

* slices can also be mutable
```rust
let mut v: Vec<i32> = vec![1,2,3,4,5,6,7];
let s: &mut[i32] = &mut v[1..];
s[0] = -2;
println!("{v:?}");  // [1, -2, 3, 4, 5, 6, 7]
```

# Exercise
Create a function that accepts a slice of floats, and returns a vector of all the partial sums of the slice.

```
part_sums(&[1,2,3,4]) == [1,3,6,10]
part_sums(&[10,9,8,7]) == [10,19,27,34]
part_sums(&[10]) == [10]
part_sums(&[]) == []
```