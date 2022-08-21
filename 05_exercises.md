# Exercise 1

We're going to implement a program that analyzes multiple strategies for single player games.

our framework will be:

```rust
trait GameState: Default{
    type Move;
    
    fn outcome(&self)->Option<f64>;
    fn do_move(self, next_move: Self::Move)->Self;
}

trait Strategy<G: GameState>: Debug{
    fn next_move(&self, state: &G) -> G::Move;
}
```

For example we'll implement the game Dice Black Jack like so:
```rust
#[derive(Default)]
struct DiceBlackJack {
    score: usize,
    folded: bool,
}

enum DiceBlackJackMove {Fold, Hit}

impl GameState for DiceBlackJack {
    type Move = DiceBlackJackMove;
    
    fn outcome(&self)->Option<f64>{
        if !self.folded{
            None
        } else {
            Some(if self.score > 21 {-21.0} else {self.score as f64 - 10.0})
        }
    }
    
    fn do_move(self, next_move: Self::Move)->Self{
        match next_move{
            Self::Move::Hit => Self{score: self.score
                                    + thread_rng().gen_range(1..=6), ..self},
            Self::Move::Fold => Self{folded: true, ..self}
        }
    }
}

#[derive(Debug)]
struct FoldAt(usize);

impl Strategy<DiceBlackJack> for FoldAt{
    fn next_move(&self, state: &DiceBlackJack) -> DiceBlackJackMove{
        if state.score >= self.0{
            DiceBlackJackMove::Fold
        } else {
            DiceBlackJackMove::Hit
        }
    }
}
```

implement a function that accepts multiple strategies for the same game, plays multiple games with each strategy, and prints the score for each.

Try to implement games and strategies for:
* the [monty hall problem](https://en.wikipedia.org/wiki/Monty_Hall_problem)
* card blackjack (with a set-aside deck)
* [Mastermind](https://en.wikipedia.org/wiki/Mastermind_(board_game))

# Exercise 2

We have a hierarchy of vertexes, each vertex has a value, and can have any number of vertex children. Multiple vertexes can have the same vertexes as children.

for example:
root vertex A has value 1 and 3 children: B, C, and D

B has value 2 and 2 children: E and F
C has value 3 and 2 children: F and D

D has value 4
E has value 5
F has value 4

implement this type `Vertex`, implement a method `depth` that, given a value to search, will return the lowest depth at which a vertex exists

following from the example above:
```
depth(A, 3) == 1
depth(A, 1) == 0
depth(A, 5) == 2
depth(A, 4) == 1
depth(A, 40) == None
``` 