let's implement some methods

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn new(width: u32, height: u32) -> Rectangle {
        Rectangle {
            width: width,
            height: height,
        }
    }

    fn area(&self) -> u32 {
        return self.height * self.width
    }

    fn perimeter(&self) -> u32 {
        return 2 * (self.height + self.width)
    }
}

fn main() {
    let rect = Rectangle::new(13, 21);
    println!("our rectangle has area {} and perimeter {}", rect.area(), rect.perimeter())
}
```

We can use some syntax sugar to make the `new` better.

```rust
impl Rectangle {
    fn new(width: u32, height: u32) -> Self {
        Self {
            width,
            height,
        }
    }
}
```

# Question
Why would we want to create a constructor in this case?