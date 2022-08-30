We can use the following "meta-variable" kinds in declarative macros (among others):
* `expr`: any expression (that has a value)
* `ty`: any type definition
* `ident`: any single-word identifier
* `path`: any multi-word qualified path (`T::func`)
* `stmt`: any variable, function, or type declaration
* `literal`: a literal expression

note that these types overlap and no semantic analysis takes place: `my_field` is:
* a valid `ident` (since it is a valid identifier)
* a valid `path` (since identifiers are paths)
* a valid `expr` (since it can be the name of a variable previously defined)
* a valid `ty` (since it can be the name of a type previously defined)

We can also include constant tokens in our macro

```rust
macro_rules! mk_transparent_type {
    (struct $name: ident( $inner: ty )) => {
        #[derive(Debug)]
        struct $name($inner);

        impl From<$inner> for $name{
            fn from(x: $inner)->Self{
                Self(x)
            }
        }

        impl From<$name> for $inner{
            fn from(x: $name)->Self{
                x.0
            }
        }
    }
}

mk_transparent_type!(struct MyVec(Vec<usize>));

fn main(){
    let my_vec = MyVec::from(vec![1,2,6,8]);
    println!("{my_vec:?}") // MyVec([1, 2, 6, 8])
}
```