For now, our generics are pretty limited, we can't make any assumptions about our generic types.

```rust
fn print_twice<T>(x: T){
    println!("{x}, {x}");  // this will fail
}
```

To allow for assumptions, we can use traits. Traits are capabilities that types can implement.

```rust
// traits are usually named after the verb that they can perform
trait Speak{
    fn speak(&self)->String;
}

struct Person{
    name: String,
}

impl Speak for Person{
    fn speak(&self)->String{
        format!("Hi! my name is {}", self.name)
    }
}

struct Dog;

impl Speak for Dog{
    fn speak(&self)->String{
        "Bark!".to_string()
    }
}
```

Now we can demand that a generic parameter implement a trait.

```rust
fn speak_backwards<T>(speaker: &T)->String where T:Speak{
    speaker.speak()
        .chars().rev().collect()
}

fn main() {
    println!("{}", speak_backwards(&Dog)) // !kraB
}
```

we can replace the complex `where` suffix with this syntactic sugar:
```rust
fn speak_backwards<T: Speak>(speaker: &T)->String {...}
```

or even shorter:

```rust
fn speak_backwards(speaker: &impl Speak)->String {...}
```

note that the last form makes it impossible to re-use the generic type, for example:

```rust
fn dialogue(speaker0: &impl Speak, speaker1: &impl Speak)->String{
    format!("{}\n{}", speaker0.speak(), speaker1.speak())
}
```

is equivalent to

```rust
fn dialogue<T0: Speak, T1: Speak>(speaker0: &T0, speaker1: &T1)->String{ ... }
```

Which might be what you want, but not always:

```rust
fn add_to_vec(v: &mut Vec<impl Speak>, speaker: impl Speak){
    v.push(speaker); // this will fail
}
```
output:
```
error[E0308]: mismatched types
  --> src/main.rs:30:12
   |
29 | fn add_to_vec(v: &mut Vec<impl Speak>, speaker: impl Speak){
   |                           ----------            ---------- found type parameter
   |                           |
   |                           expected type parameter
30 |     v.push(speaker);
   |            ^^^^^^^ expected type parameter `impl Speak`, found a different type parameter `impl Speak`
   |
   = note: expected type parameter `impl Speak` (type parameter `impl Speak`)
              found type parameter `impl Speak` (type parameter `impl Speak`)
```

# Exercise
Implement a function that counts the empty values in a vector. The function should be able to accept vectors of the following types:
* `usize`, where 0 is empty
* `String`, where `""` is empty

# Exercise 2 (advanced)
also implement the function for the following item size
* `Vec<?>` of any type, where `[]` is empty