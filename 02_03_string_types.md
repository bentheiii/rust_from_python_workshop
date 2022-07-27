* rust had two distinct string types:
* `String`, which can be thought of as a vector of chars.
* `&str`, A.K.A "string slice", is a reference to a part of a `String`.
* Literals always evaluate to `&str` because the compiler makes no promises about where the `String` will be.

```rust
let s1 = "banana";
let s2 = "banana";
let p1 = s1.as_ptr();
let p2 = s2.as_ptr();
assert_eq!(p1, p2);
```

# Exercise 1
create a function that accepts a string slice, and returns whether it is a palindrome.

# Exercise 2
Implement the function in one line.
