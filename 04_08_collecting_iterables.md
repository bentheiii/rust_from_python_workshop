In order to convert an iterable like vector or set to an iterator we can use the `iter` method. To convert an iterator of elements back into a concrete collection we can use the `collect` method.

```rust
let harmonics = (1..10).map(|i| 1.0/(i as f64)).collect();  // this will fail
```

Just like in our `default_plus_two` example, we need to make it clear to the compiler what `harmonics` should be, either with explicit typing, type interpolation, or the turbofish `::<>`

```rust
let harmonics = (1..10).map(|i| 1.0/(i as f64)).collect::<Vec<f64>>();
println!("{harmonics:?}")
// [1.0, 0.5, 0.3333333333333333, 0.25, 0.2, 0.16666666666666666, 0.14285714285714285, 0.125, 0.1111111111111111]
```

Just like before, we don't have to specify the whole type.

```rust
let harmonics: Vec<_> = (1..10).map(|i| 1.0/(i as f64)).collect();
```

`collect` has a useful trick where it can aggregate an iterable of `Option`s or `Result`s.

```rust
let users = vec!["Jerry", "Barry", "Garry", "Larry"];

fn get_address(user: &str)->Result<String, String>{
    if user == "Garry"{
        Err("Garry has no email".to_string())
    } else {
        Ok(format!("{user}.Gergich@gmail.com"))
    }
}

let addresses_result: Result<Vec<_>, _> = users.iter().map(|&user| get_address(user)).collect();

println!("{addresses_result:?}");  // Err("Garry has no email")
```