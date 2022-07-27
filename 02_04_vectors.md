* A vector `Vec` is rust's equivalent of a dynamic array (list).
* It is a generic type, meaning that it's type accepts another type as a parameter.
* We construct it with the `vec!` macro

```rust
let x: Vec<i32> = vec![1,2,3,4];
let y: Vec<Vec<i32>> = vec![vec![1], vec![1,1], vec![2,1], x];
assert_eq!(y.len(), 4);
```

# Exercise 1
Implement a function that accepts a number and a base, and returns a vector of the digits of that number in the base

```
digits(159, 10) == [9, 5, 1]
digits(159, 2) == [1, 1, 1, 1, 1, 0, 0, 1]
digits(159, 20) == [19, 7]
```