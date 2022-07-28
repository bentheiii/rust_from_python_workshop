implementations for our traits can be generic too
```rust
trait Color{
    fn color(&self)->String;
}

struct Hedgehog;

// specialized implementation
impl Color for Hedgehog {
    fn color(&self) -> String{
        "blue".to_string()
    }
}

// blanket implementation
impl<T: Color> Color for Option<T> {
    fn color(&self) -> String{
        match &self{
            Some(inner)=>inner.color(),
            None => "missing".to_string()
        }
    }
}



println!("{}", Hedgehog.color()); // blue

let maybe: Option<Hedgehog> = None;
println!("{}", maybe.color()); // missing
```

we can also require multiple traits in out generic requirements with the `+` operator

```rust
trait Walk {
    fn walk(&self);
    fn stop_walking(&self);
}

trait Talk {
    fn talk(&self)->String;
}

trait Listen {
    fn listen(&self, words: String);
}

fn moving_conversation<S, L>(speaker: S, listener: L) where
 S: Walk + Talk,
 L: Walk + Listen {
    speaker.walk();
    listener.walk();
    listener.listen(speaker.talk());
    listener.stop_walking();
    speaker.stop_walking();
 }
```

our traits can have default implementations for the most common use cases.

```rust
trait Talk{
    fn talk(&self)->String;
    fn yell(&self)->String{
        format!("{}!",self.talk().to_ascii_uppercase())
    }
}

struct Person;

impl Talk for Person{
    fn talk(&self)->String{
        "hi there".to_string()
    }
}

struct Cat;

impl Talk for Cat{
    fn talk(&self)->String{
        "meow".to_string()
    }
    fn yell(&self)->String{
        "HISS".to_string()
    }
}

fn escalate<T: Talk>(speaker: &T){
    println!("{}... {}", speaker.talk(), speaker.yell());
}

escalate(&Person); //  hi there... HI THERE!
escalate(&Cat); // meow... HISS
```

finally, our traits can request that other traits be implemented for a type

```rust
trait Talk{
    fn talk(&self)->String;
}

trait Yell: Talk{ // anyone who implements Yell, must also implement Talk
    fn yell(&self)->String{
        format!("{}!",self.talk().to_ascii_uppercase())
    }
}
```