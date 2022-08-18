There is a special reserved lifetime called the `static` lifetime. The static lifetime is for data that we know will never be deleted for the entire program.

```rust
let foo: &'static str = "aaa";
```

We can also create static lifetimes with `const` and `static` variables

```rust
fn my_function(x: &'static usize){}

const MY_CONST: usize = 1000;
static MY_STATIC: usize = 15;

fn main(){
    my_function(&MY_CONST);
    my_function(&MY_STATIC);
    
    let local = 3usize;
    my_function(&local);  // will fail
}
```