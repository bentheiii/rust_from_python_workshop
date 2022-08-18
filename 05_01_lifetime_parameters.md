If you ever tried you make a struct that includes a reference, you might have run into trouble

```rust
struct Book{
    title: String,
    text: &str, // this will fail
}
```

```output
error[E0106]: missing lifetime specifier
 --> src/lib.rs:3:11
  |
3 |     text: &str, // this will fail
  |           ^ expected named lifetime parameter
  |
help: consider introducing a named lifetime parameter
```

This is because rust has to guarantee that the reference inside `Book` will never outlive the string that is being referenced at `Book.text`.

```rust
fn mk_book()->Book{
    let text = "my text".to_string();
    return Book{
        title: "my_book".to_string(),
        text: &text,
    }
}  // text will be deleted here, but book will remain
```

To solve this, we introduce a special lifetime variable to the type

```rust
struct Book<'a>{
    title: String,
    text: &'a str, // this will demand that the reference be valid for a lifetime 'a
}
```

Now `Book` is generic over the lifetime `'a`, and whenever `Book` is used, the lifetime must be specified as a generic variable

```rust
fn trim_dots<'a>(book: &Book<'a>)->&'a str{
    book.text.trim_matches('.')
}

fn book_length<'a>(book: &Book<'a>)->usize{
    book.text.len()
}
```

Or as a method


```rust
impl<'a> Book<'a>{
    fn len(&self)->usize{
        self.text.len()
    }
}
```

A lot of the time, lifetimes can be anonymized or even elided

```rust
fn book_length(book: &Book)->usize{
    book.text.len()
}

impl Book<'_>{
    fn len(&self)->usize{
        self.text.len()
    }
}
```