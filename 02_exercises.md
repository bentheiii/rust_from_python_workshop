# Exercise 1
Create a function that returns a vector of a number's prime divisors

# Exercise 2
given the following enum
```rust
enum Nested {
    Value(i32),
    List(Vec<NestedList>),
}
```

create a function that returns a `Nested` with json encoding

```
to_json(Nested::List(vec![Nested::List(vec![]), Nested::List(vec![Nested::Value(1), Nested::Value(2)]), Nested::Value(3)])) == "[[], [1,2], 3]"
```

# Exercise 3
Create a struct of a tic-tac-toe game. The game should allow both players to place pieces on the board, and check for game end.

# Exercise 4
Continuing from exercise 3, create a function/struct that can play the optimal tic-tac-toe move given a board.