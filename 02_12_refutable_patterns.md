* so far we covered irrefutable patterns, but what about patterns that might not be possible?
* The simplest of these is the const pattern
  ```rust
  fn foo(i: i32){
    let 0 = i;
    ...
  }
  ```
* in order to allow for the possibility of a pattern not matching, we need to use `if let`.
  ```rust
  let x = 0;
  if let 1 = x {
    println!("this won't run")
  } else if let 0 = x {
    println!("this will run")
  }
  ```
* like all patterns, const patterns can be nested
  ```rust
  fn sum_of_squares(point: [i64; 3])->i64{
    if let [x, 0, 0] = point {
        x*x
    } else if let [x, y, 0] = point {
        x*x + y*y
    } else {
        let [x, y, z] = point ;
        x*x + y*y + z*z
    }
  }
  ```
* A useful refutable pattern is to get a range
  ```rust
  fn abs(i: i32)->u32{
    if let 0..=i32::MAX = i{
      i as u32
    } else{
      -i as u32
    }
  }
  ```
* Another useful refutable pattern is to unpack a slice
  ```rust
  fn is_palindrome(arr: &[i32])->bool {
    if let [a,..,b] = arr{
        a == b && is_palindrome(&arr[1..arr.len()-1])
    } else {
        true
    }
  }
  ```
* Finally, we can match enum variants refutably
  ```rust
  enum RoadSign{
    Stop,
    SpeedLimit {maximum_speed: u8},
    Yield,
  }

  impl RoadSign{
    fn shape(&self)->String{
        if let RoadSign::Stop = self{
            "octagon"
        } else if let RoadSign::SpeedLimit {maximum_speed: _} = self{
            "circle"
        } else {
            "triangle"
        }.to_string()
    }

    fn should_slow(&self, current_speed: u8)->bool{
        if let Self::Stop | Self::Yield = self{
            true
        } else if let Self::SpeedLimit {maximum_speed} = self{
            *maximum_speed < current_speed
        } else {
            unreachable!()
        }
    }
  }
  ```
# Exercise 
Given the following enum
```rust
enum Nested{
    Value(i32),
    Array(Vec<Nested>),
}
```

implement a function `unnest` that returns a single `Vec<i32>` that contains the flattened elements of a `Nested`