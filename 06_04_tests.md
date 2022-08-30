adding unittests in rust is easy with the `#[test]` proc macro
```rust
fn longest_common_prefix<T, U>(item0: T, item1: T)->U
where T:IntoIterator, U:FromIterator<T::Item>, T::Item: PartialEq
{
    item0.into_iter()
    .zip(item1.into_iter())
    .map_while(|(a,b)| (&a==&b).then_some(a))
    .collect()
}

#[test]
fn check_ban(){
    let s: String = longest_common_prefix("banana".chars(), "bane, lord of hells".chars());
    assert_eq!("ban", s)
}
```

Output

```
running 1 test
test check_ban ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s
```

unit tests are usually stored at the end of the file of what they're testing. While integration tests are usually stored in a separate directory.