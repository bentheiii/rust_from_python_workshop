traits can also require types to have constant values. These are called "Associated Constants".

```rust
trait Atom{
    const ATOMIC_NUMBER: u8;
}

fn describe<T: Atom>() -> String {
    format!("atomic number {}", T::ATOMIC_NUMBER)
}

struct Carbon;
struct Uranium;

impl Atom for Carbon{
    const ATOMIC_NUMBER: u8 = 6;
}

impl Atom for Uranium{
    const ATOMIC_NUMBER: u8 = 92;
}

fn main() {
    println!("{}", describe::<Carbon>());  // atomic number 6
}
```

Most of the time, associated constants aren't very useful, but they are a good opening for a concept called "Associated Types"

```rust
trait Half{
    type T;

    fn half(&self)->Self::T;
}

impl Half for i32{
    type T = f64;
    fn half(&self)->Self::T{
        let f: f64 = (*self).into();
        f/2.0
    }
}

impl Half for String{
    type T = String;
    fn half(&self)->Self::T{
        self[..self.len()/2].to_string()
    }
}
```

Note that associated types is not generic traits, and in fact a generic trait can have associated types.

```rust
struct Time{...};
struct TimeDelta{...};

impl Add<Time> for TimeDelta{
    type Output = Time

    dn add(self, other: Time) -> Self::Output {...}
}

impl Add<TimeDelta> for TimeDelta{
    type Output = TimeDelta

    dn add(self, other: Time) -> Self::Output {...}
}
```