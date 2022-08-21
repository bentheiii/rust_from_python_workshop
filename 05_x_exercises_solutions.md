# Exercise 1
```rust
fn play<G: GameState>(strategy: &Box<dyn Strategy<G>>)->f64{
    let mut state = G::default();
    loop {
        if let Some(ret) = state.outcome(){
            break ret;
        } else {
            let next_move = strategy.next_move(&state);
            state = state.do_move(next_move);
        }
    }
}

fn average<G: GameState>(strategy: &Box<dyn Strategy<G>>, sample_size: usize)->f64{
    let mut total = 0.0;
    for _ in 0..sample_size{
        total += play(strategy);
    }
    total / (sample_size as f64)
}

fn analyze<G: GameState>(strategies: &[Box<dyn Strategy<G>>], sample_size: usize){
    for strat in strategies{
        println!("{:?}: {}", strat.as_ref(), average(strat, sample_size))
    }
}
```

# Exercise 2

```rust
struct Vertex<'a, T>{
    value: T,
    children: Vec<&'a Self>
}

impl<'a, T> Vertex<'a, T>{
    fn leaf(value: T)->Self{
        Self{value, children: vec![]}
    }
    
    fn root(value: T, children: Vec<&'a Self>)->Self{
        Self{value, children}
    }
}

impl<T: PartialEq<T>> Vertex<'_, T>{
    fn depth(&self, needle: &T)->Option<usize>{
        if &self.value == needle{
            Some(0)
        } else {
            self.children.iter().filter_map(|c| c.depth(needle)).min().map(|a| a+1)
        }
    }
}
```