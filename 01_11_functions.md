* functions are very similar to what we know from python, we create them with the `fn` keyword.
  ```rust
  fn speak(){
      println!("Hello!");
  }
  ```
* since rust is a statically typed language, all our parameters, as well as our output type, if any, must be specified in our signature.
  ```rust
  fn square(x: i32)->i32 {
    return x*x;
  }
  ```
* note that our parameters are variables and are thus immutable by default, we can make the variable mutable with `mut`
  ```rust
  fn odd_divisor(mut x: i32)->i32 {
    // remember that x is on the stack, just like any other variable
    while x%2 == 0{
        x /= 2;
    }
    return x;
  }
  ```

# Excercise
Create a function "seven_boom", that returns `true` if a number is divisible by 7 or has 7 in its digits.