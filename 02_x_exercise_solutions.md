# Exercise 1

```rust
fn next_divisor(x: usize, mut next_to_check: usize)->Option<usize>{
    loop {
        let div = x / next_to_check;
        let rem = x % next_to_check;
        if rem == 0 {
            return Some(next_to_check)
        } else if div < next_to_check {
            return None
        } else {
            next_to_check += 2;
        }
    }
}

fn prime_divisors(mut x: usize)->Vec<usize>{
    let mut ret = vec![];
    while x%2 == 0 {
        x /= 2;
        ret.push(2);
    }
    let mut candidate = 3;
    while let Some(divisor) = next_divisor(x, candidate){
        ret.push(divisor);
        x /= divisor;
        candidate = divisor;
    }
    if x != 1{
        ret.push(x);
    }
    ret
}
```

# Exercise 2
```rust
enum Nested {
    Value(i32),
    List(Vec<Nested>),
}

fn to_json(value: &Nested)->String {
    match value{
        Nested::Value(i) => format!("{i}"),
        Nested::List(items) => {
            let mut ret = String::from("[");
            let mut is_first = true;
            for item in items{
                if !is_first{
                    ret.push_str(", ");
                } else {
                    is_first = false;
                }
                ret.push_str(&to_json(&item));
            }
            ret.push_str("]");
            ret
        }
    }
}
```

# Exercise 3 & 4

```rust
use std::collections::VecDeque;

#[derive(Clone, Debug, Default, Copy, PartialEq, Eq)]
enum Mark{#[default] X, O}

impl Mark{
    fn flip(&self)->Self{
        match self{
            Self::X=>Self::O,
            Self::O=>Self::X,
        }
    }
}

#[derive(Clone, Debug, Default)]
struct Board([[Option<Mark>;3];3], Mark);

type Move = (usize, usize);

#[derive(Debug)]
struct OptimalMove{
    moves: VecDeque<Move>,
    score: i8,
}

impl OptimalMove{
    fn push_back(mut self, first_move: Move)->Self{
        self.moves.push_front(first_move);
        self.score = -self.score;
        self
    }
    
    fn is_better_than(&self, other: &Self)->bool{
        if self.score != other.score {
            self.score > other.score
        } else{
            if self.score >= 0{
                self.moves.len() < other.moves.len()
            } else {
                self.moves.len() > other.moves.len()
            }
        }
    }
}

impl Board {
    fn victor(&self)->Option<Mark> {
        fn check_line(x:Option<Mark>,y:Option<Mark>,z:Option<Mark>)->Option<Mark>{
            match (x,y,z){
                (Some(x), Some(y), Some(z)) if x == y && y == z => Some(x),
                _=>None
            }
        }
        
        // check all horizontals
        for [a,b,c] in self.0{
            let res = check_line(a,b,c);
            if res.is_some(){
                return res;
            }
        }
        // check all verticals
        for i in 0..3{
            let res = check_line(self.0[0][i], self.0[1][i], self.0[2][i]);
            if res.is_some(){
                return res;
            }
        }
        check_line(self.0[0][0], self.0[1][1], self.0[2][2])
        .or(check_line(self.0[2][0], self.0[1][1], self.0[0][2]))
    }
    
    fn play(&self, (i,j):Move)->Self{
        assert!(self.0[i][j].is_none());
        let mut ret = self.clone();
        ret.0[i][j] = Some(ret.1);
        ret.1 = ret.1.flip();
        ret
    }
    
    fn next_moves(&self)->Vec<Move>{
        let mut ret = vec![];
        for i in 0..3{
            for j in 0..3{
                if self.0[i][j].is_none(){
                    ret.push((i,j))
                }
            }
        }
        return ret
    }
    
    fn optimal_move(&self)->OptimalMove{
        let next_moves = self.next_moves();
        if next_moves.is_empty(){
            return OptimalMove{moves: VecDeque::new(), score: 0}
        }
        let mut ret = OptimalMove{moves: VecDeque::new(), score: i8::MIN};
        for next_move in next_moves{
            let next_board = self.play(next_move);
            let opt = if let Some(victor) = next_board.victor(){
                OptimalMove{moves: VecDeque::from([next_move]),
                            score: if victor == self.1 {1} else {-1}}
            } else {
                next_board.optimal_move().push_back(next_move)
            };
            if opt.is_better_than(&ret){
                ret = opt;
            }
        }
        ret
    }
}
```