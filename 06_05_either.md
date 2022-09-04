Sometimes we want to hold a value, but it might be of two different unrelated types.

```rust
fn foo<T: PartialEq>(it: impl IntoIterator<Item=T>, remove: Option<T>)->impl Iterator<Item=T>{
    let iter = it.into_iter();
    if let Some(item_remove) = remove{
        iter.filter(move |i| i != &item_remove)  // this will fail
    } else {
        iter
    }
}
```

output:
```
error[E0308]: `if` and `else` have incompatible types
 --> src/lib.rs:8:9
  |
5 | /     if let Some(item_remove) = remove{
6 | |         iter.filter(move |i| i != &item_remove)  // this will fail
  | |         ---------------------------------------
  | |         |           |
  | |         |           the expected closure
  | |         expected because of this
7 | |     } else {
8 | |         iter
  | |         ^^^^ expected struct `Filter`, found associated type
9 | |     }
  | |_____- `if` and `else` have incompatible types
  |
  = note:       expected struct `Filter<<impl IntoIterator<Item = T> as IntoIterator>::IntoIter, [closure@src/lib.rs:6:21: 6:47]>`
          found associated type `<impl IntoIterator<Item = T> as IntoIterator>::IntoIter`
```

This is because `Filter` and `Iter` are different types. In order to allow a variable of one of any types, we can use the `Either` enum.

```rust
enum Either<L,R>{
    Left(L),
    Right(R),
}
```

Either has a bunch of traits implemented if both their sides implement it

```rust
impl<T, L, R> Iterator for Either<L,R> 
where L: Iterator<Item=T>, R: Iterator<Item=T>{
    ...
}
```

so we can implement this function like so
```rust
fn foo<T: PartialEq>(it: impl IntoIterator<Item=T>, remove: Option<T>)->impl Iterator<Item=T>{
    let iter = it.into_iter();
    if let Some(item_remove) = remove {
        Either::Left(iter.filter(move |i| i != &item_remove))
    } else {
        Either::Right(iter)
    }
}
```