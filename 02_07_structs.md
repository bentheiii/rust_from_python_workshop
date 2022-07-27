* A struct is a collection of variables bundled together
```rust
struct Person{
    name: String,
    email: String,
    yob: i16,
}

let pete = Person{
    name: "pete".to_string(),
    email: "somemail@pete.pete".to_string(),
    yob: 1997,
};

println!("{}'s email is {} and he was born at {}", pete.name, pete.email, pete.yob);
// pete's email is somemail@pete.pete and he was born at 1997

struct Color(u8,u8,u8);

let deadbe = Color(222,173,190);

println!("deadbe's coordinates are {}, {}, {}", deadbe.0, deadbe.1, deadbe.2);
// deadbe's coordinates are 222, 173, 190

struct Trivial;

let triv = Trivial;
```

# Exercise
Create struct that represents a rectangle. Implement two functions, one to create a rectangle with a width and height, and one to calculate a given rectangle's area.