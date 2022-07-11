* In python, all variables exist in the heap, this is because their type, size, or even existence cannot be determined before the scope is run. This is why we call it a dynamic language.
* Rust is a static language, the type of all variables are known even before the program is run.

```rust
fn foo() {
    let x: i32 = 1;
    // while the array itself might shrink and grow, the stack variable (a pointer to the array) is constant sized
    let list0: Vec<f64> = vec![];
    let arr: [u8; 100] = [0;100];  // allocated on the stack!
    let list1 = vec![3, 9, 39];  // the type is inferred to be Vec<i32>
    let list2 = vec![9, 36.0] // this will fail compilation!
}
```