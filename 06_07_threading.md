Rust makes concurrency with threads easy and safe, we can create new threads with the `spawn` function

```rust
let my_thread = thread::spawn(|| {
    for i in 1..10 {
        println!("hi number {} from the spawned thread!", i);
        thread::sleep(Duration::from_millis(1));
    }
});

for i in 1..5 {
    println!("hi number {} from the main thread!", i);
    thread::sleep(Duration::from_millis(1));
}

my_thread.join().unwrap();
```
Output:
```
hi number 1 from the main thread!
hi number 1 from the spawned thread!
hi number 2 from the main thread!
hi number 2 from the spawned thread!
hi number 3 from the spawned thread!
hi number 3 from the main thread!
hi number 4 from the spawned thread!
hi number 4 from the main thread!
hi number 5 from the spawned thread!
hi number 6 from the spawned thread!
hi number 7 from the spawned thread!
hi number 8 from the spawned thread!
hi number 9 from the spawned thread!
```

Due to ownership rules: sharing data across threads is more complicated (but safer) than in other low-level languages.

```rust
const N: usize = 6;
    
let shared_state = Arc::new(Mutex::new([None; N]));
let (tx, rx) = mpsc::channel(); // messaging
let mut threads = vec![];
for i in 0..N{
    let thread = {
        let thread_state = shared_state.clone();
        let thread_transmitter = tx.clone();
        thread::spawn(move || {
            thread::sleep(Duration::from_millis(rand::random::<u64>() % 1000));
            let num = rand::random::<usize>() % 36 + 1;
            {
                let mut locked_data = thread_state.lock().unwrap();
                locked_data[i] = Some(num);
            }
            thread_transmitter.send(i).unwrap();
        })
    };
    threads.push(thread);
}

fn print_state(state: &[Option<usize>]){
    println!("{}", state.iter().map(|i| i.map_or_else(|| "?".to_string(), |v| v.to_string())).join(", "));
}

for _ in 0..N{
    let idx: usize = rx.recv().unwrap();
    println!("updated index: {idx}");
    {
        let locked_data = shared_state.lock().unwrap();
        print_state(locked_data.as_ref());
    }
}

for thread in threads{
    thread.join().unwrap();
}
```

output:
```
updated index: 3
?, ?, ?, 14, ?, ?
updated index: 4
?, ?, ?, 14, 4, ?
updated index: 5
?, ?, ?, 14, 4, 29
updated index: 1
?, 29, ?, 14, 4, 29
updated index: 2
?, 29, 28, 14, 4, 29
updated index: 0
11, 29, 28, 14, 4, 29
```