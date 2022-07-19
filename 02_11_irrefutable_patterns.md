* we can use patterns in `let` statements to unpack values
  ```rust
  fn distance(point0: (f64, f64), point1: (f64, f64))->f64{
      let (x0, y0) = point0;
      let (x1, y1) = point1;
      let xd = x0-x1;
      let yd = y0-y1;
      (xd*xd + yd*yd).sqrt()
  }
  ```
* we can unpack structs and arrays as well!
  ```rust
  fn brg_to_rgb(color: [i8; 3])->[i8; 3]{
    let [b,r,g] = color;
    [r,g,b]
  }

  struct Person {
    first_name: String,
    surname: String,
    title: String,
  }

  impl Person{
    fn full_name(&self)->String{
        let Person{first_name, surname, title: my_title} = self;
        format!("{my_title} {first_name} {surname}")
    }
  }
  ```
* we can also use `_` and `..` as wildcards in our patterns
  ```rust
  struct Person{
    first_name: String,
    surname: String,
    title: String,
    year_of_birth: u32,
  }

  impl Person{
    fn full_name(&self)->String{
        let Person{first_name, surname, title: my_title, year_of_birth: _} = self;
        format!("{my_title} {first_name} {surname}")
    }

    fn age(&self)->usize{
        let Person{year_of_birth, ..} = self;
        current_year() - year_of_birth()
    }
  }

  fn mid(arr: [u64; 8])->u64{
    let [first, .., last] = arr;
    (first + last) / 2
  }
  ```