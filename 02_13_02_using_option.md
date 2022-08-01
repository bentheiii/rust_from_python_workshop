Option has some methods we need to know for now.

* `is_none`: returns `true` if the value is `None`
* `is_some`: returns `true` if the value is not `None`
* `unwrap`: unpack the inner value, or `panic` if it is `None`
* `expect`: like `unwrap`, but panics with a specific error message.
* `unwrap_or(default)`: returns the value if it exists or `default` if the value is `None`.
* `ok_or(err)`: convert a `Some` value to an `Ok`, and a `None` to an `Err(err)`.
* `and(optb)`, `or(optb)`, `xor(optb)`, logical and/or/xor, treats `None` values as falsish.