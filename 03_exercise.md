# Exercise 1
Create a framework for handling 3x3 matrices of a generic value. The declaration might look something like:
```rust
struct Matrix<T>{
    values: Vec<Vec<T>>, // NOTE: can you do this better?
}
```

Matrices should support:
* `Clone`, `Eq`, `PartialEq`, `Ord`, `PartialOrd`, `Hash`, `Debug`, `Display` (if possible)
* Adding and subtracting two matrices by value.
* Multiplying a matrix with a scalar.
* Multiplying two matrixes with matrix multiplication.
* Method to transpose a matrix
* Advanced: implement the `num::One` and `num::Zero` traits for a matrix.
* Advanced: method to calculate the determinant of a matrix
* Advanced: method to invert a matrix

# Exercise 2
We're going to make a generic puzzle solving machine. For this we're going to use the following declarations

```rust
enum GameResult{Victory, Loss, Undecided}

trait GameState{
    type Move;

    fn possible_moves(&self)->Vec<Self::Move>;
    fn do_move(&self, next_move: Self::Move)->Self;
    fn result(&self)->GameResult;
}
```

A possible usage of this when solving the [fox-chicken-grain](https://www.mathsisfun.com/chicken_crossing_solution.html) puzzle:
```rust
#[derive(Clone, Debug, Default)]
struct FoxChickenGrainState{
    fox_crossed: bool,
    chicken_crossed: bool,
    grain_crossed: bool,
    man_crossed: bool,
}

#[derive(Clone, Debug)]
enum FoxChickenGrainMove {
    MoveFox,
    MoveChicken,
    MoveGrain,
    MoveNothing,
}

impl GameState for FoxChickenGrainState{
    type Move = FoxChickenGrainMove;
    
    fn result(&self) -> GameResult{
        if self.fox_crossed && self.chicken_crossed && self.grain_crossed && self.man_crossed{
            return GameResult::Victory;
        }
        let fox_alone = self.fox_crossed != self.man_crossed;
        let chicken_alone = self.chicken_crossed != self.man_crossed;
        let grain_alone = self.grain_crossed != self.man_crossed;
        if chicken_alone && (fox_alone || grain_alone){
            GameResult::Loss
        } else {
            GameResult::Undecided
        }
    }

    fn possible_moves(&self)->Vec<Self::Move>{
        let mut ret = vec![Self::Move::MoveNothing];
        if self.fox_crossed == self.man_crossed{
            ret.push(Self::Move::MoveFox);
        }
        if self.chicken_crossed == self.man_crossed{
            ret.push(Self::Move::MoveChicken);
        }
        if self.grain_crossed == self.man_crossed{
            ret.push(Self::Move::MoveGrain);
        }
        ret
    }

    fn do_move(&self, next_move: Self::Move)->Self{
        let mut ret = self.clone();
        ret.man_crossed ^= true;
        match next_move{
            Self::Move::MoveFox => ret.fox_crossed ^= true,
            Self::Move::MoveChicken => ret.chicken_crossed ^= true,
            Self::Move::MoveGrain => ret.grain_crossed ^= true,
            _ => {}
        };
        ret
    }
}
```

Implement a function that accepts an initial game state and returns the shortest vector of moves that bring it to a winning state.

try to implement this trait for other games like:

* [the 4 liter puzzle](https://www.mathsisfun.com/puzzles/measuring-4-liters-solution.html)
* [the towers of hanoi](https://en.wikipedia.org/wiki/Tower_of_Hanoi)
* a small 3x3 sudoku puzzle