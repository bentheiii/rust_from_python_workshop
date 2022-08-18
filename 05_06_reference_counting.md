Sometimes we don't know when a value should be deleted, and we want to share it across different owners. We do this with `Rc<T>`.

`Rc`, short for "Reference Counted", is a smart pointer to a value on the heap that can count how many variables are referring to it at any time. In this way we can hold multiple references to the same data on the heap, and maintain memory safety.

```rust
{
    let rc0 = Rc::new([0;1000]); // reference count initialized to 1
    let rc2 = rc0.clone()  // reference count increased to 2, no copying happens
    {
        let rc1 = rc0.clone();  // reference count increased to 3, no copying happens
    }  // reference count decreased to 2
    mem::drop(rc0)  // reference count decreased to 1
}  // reference count decreased to 0, data is deleted
```

We can also use `rc::Weak` to keep a reference to an `Rc` without increasing the reference count.