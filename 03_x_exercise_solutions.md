# Exercise 2
```rust
fn solve<S: GameState>(initial: S) -> Vec<S::Move> where S::Move : Clone{
    struct Path<S: GameState>(S, Vec<S::Move>);
    
    let mut paths = VecDeque::from([Path(initial, vec![])]);
    while let Some(Path(state, moves)) = paths.pop_front(){
        match state.result(){
            GameResult::Victory => return moves,
            GameResult::Loss => continue,
            GameResult::Undecided => {
                for next_move in state.possible_moves(){
                    let next_state = state.do_move(next_move.clone());
                    paths.push_back(
                        Path(next_state, 
                             moves.iter().cloned().chain(iter::once(next_move)).collect()
                        )
                    );
                }
            }
        }
    }
    panic!("victory is unreachable")
}
```