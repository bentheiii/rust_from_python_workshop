Another useful construct is the `while let` loop

```rust
fn print_messages(mut messages: Vec<&str>){
    while let Some(msg) = messages.pop(){
        println!("new message: {}", msg)
    }
}
```