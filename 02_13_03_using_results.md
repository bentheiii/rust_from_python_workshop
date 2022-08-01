Result has some methods we need to know for now.

* `is_ok`/`is_err`
* `ok` converts an `Ok` value into a `Some`, and an `Err` into a `None`
* `err` converts an `Err` value into a `Some`, and an `Ok` into a `None`
* `unwrap`/`expect`, get the `Ok` value, or `panic`s on `Err`
* `unwrap_or`
* `and`/`or`
* the `?` operator, allows to easily unpack an `Ok` result, or return an `Err`. Only usable in functions that return a result of the same type

```rust
expr?
```

is equivalent to 
```rust
match expr{
    Ok(value) => value,
    Err(err) => {
        return Err(err);
    }
}
```

Example:

```rust
fn get_address(user_id: UserId)->Option<EmailAddress> {...}
fn render_email(user_id: UserId)->Result<Html, String> {...}
fn send_email(destination: EmailAddress, message_content: Html)->Result<(), String> {...}

fn do_email(user_id: UserId)->Result<(), String>{
    let rendered: Html = render_email(user_id)?;
    let address: EmailAddress = get_address(user_id).ok_or(format!("user without address: {user_id}"))?;
    send_email(address, rendered)
}
```