* Sometimes we want to implement something that doesn't depend a specific payload's type.
  ```rust
  fn select_i32(a: i32, b: i32, idx: u8)->i32{
    if idx == 0{
        a
    } else {
        b
    }
  }

  fn select_string(a: String, b: String, idx: u8)->String{
    if idx == 0{
        a
    } else {
        b
    }
  }

  fn select_float(a: f64, b: f64, idx: u8)->f64{
    if idx == 0{
        a
    } else {
        b
    }
  }
  ```
* we can implement this easily with generics
  ```rust
  fn select<T>(a: T, b: T, idx: u8)->T{
    if idx == 0{
        a
    } else {
        b
    }
  }
  ```
* generics can also include the type in other generics
  ```rust
  fn some_or_default<T>(item: Option<T>, default: T)->T{
    if let Some(v) = item{
        v
    } else {
        default
    }
  }
  ```
* we can also use generic in structs and enums
  ```rust
  enum Nested<T>{
    List(Vec<Nested<T>>),
    Scalar(T),
  }

  impl<T> Nested<T>{
    fn flatten(self)->Vec<T>{
        match self{
            Self::List(lst) => {
                let mut ret = vec![];
                for i in lst{
                    ret.append(&mut i.flatten());
                }
                ret
            },
            Self::Scalar(value) => vec![value]
        }
    }
  }
  ```
* we can also make impls for specific specializations
  ```rust
  impl Nested<usize>{
    fn total(&self)->usize{
        match self{
            Self::List(lst) => {
                let mut ret = 0;
                for i in lst{
                    ret += i.total();
                }
                ret
            },
            Self::Scalar(value) => *value
        }
    }
  }
  ```