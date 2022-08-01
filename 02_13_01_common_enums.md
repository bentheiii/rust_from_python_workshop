* let's talk about two very common enums in the standard library: `Option<T>` and `Result<T,E>`
* `Option<T>` refers to something which might be missing
  ```rust
  enum Option<T>{
    Some(T),
    None
  }
  ```
* `Result<T, E>` refers to a value which might have failed to calculate
  ```rust
  enum Result<T, E>{
    Ok(T),
    Err(E)
  }
  ```
* these two enums are so common their variants can be used without their enum names!
  ```rust
  fn first_positive(arr: &[i32])->Option<i32>{
    for item in arr{
        if *item > 0{
            return Some(*item);
        }
    }
    None
  }

  fn parse_int(s: &str)->Result<usize, String>{
    let mut ret = 0;
    for c in s.chars(){
        if let Some(digit) = c.to_digit(10){
            ret = ret*10 + (digit as usize)
        } else {
            return Err(format!("character {c} is not a digit"))
        }
    }
    Ok(ret)
  }

  fn send_email(...) -> Result<(), String>{
    ...
  }
  ```

  # Exercise
  Given a slice of `i32`s, implement a function that finds the square root of the first even number in the slice. If the first even number is not positive, an error should return. If there are no even numbers in the slice, return `None`.