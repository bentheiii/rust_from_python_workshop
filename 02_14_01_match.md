* in the past we have done things like this
  ```rust
  enum Foo{
    A(i32),
    B(f64),
  }

  fn my_func(foo: &Foo){
    if let Foo::A(i) = foo{
        println!("int: {i}");
    } else if let Foo::B(f) = foo{
        println!("float: {f}");
    } else {
        unreachable!()
    }
  }
  ```
* we can avoid having to use `unreachable!` with the `match` statement
  ```rust
  fn my_func(foo: &Foo){
    match foo{
      Foo::A(i) => println!("int: {i}"),
      Foo::B(f) => println!("float: {f}")
    };
  }
* just like other scopes, `match` can return a value
  ```rust
  fn remainder(foo: &Foo)->f64{
    let ret = match foo{
      Foo::A(_) => 0.0,
      Foo::B(f) => f%1.0
    };
    ret
  }
  ```
* matches can also include guards
  ```rust
  fn is_palindrome(arr: &[i32])->bool{
    match arr{
      [a, .., b] if a == b => is_palindrome(&arr[1..arr.len()-1]),
      [_] | [] => true,
      _ => false,
    }
  }
  ```
* note: `match` statements must cover all possibilities
  ```rust
  fn sign(n: i32)->i32{
    match n{
      1..=i32::MAX => 1,
      i32::MIN..=-1 => -1
      // this won't work
    }
  }

  fn is_even(n: i32)->bool{
    match n{
      a if a%2 == 0 => true,
      a if a%2 != 0 => false,
      // neither will this
    }
  }
  ```

# Exercise
Given the types
```rust
enum DeveloperRank{ Junior, Senior }
enum Person{
  Developer{user_tag: String}
  Researcher{title: String, first_name: String, last_name:String}
  Devops{name: String, rank: DeveloperRank}
}
```

developers drink coffee, researchers drink tea, devops drink water if they're juniors and soda if they're seniors.

Implement a function that, given a slice of `Person`s, return a vector of strings with drink orders of all of them.