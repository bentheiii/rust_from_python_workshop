Async is relatively new in rust, but still very powerful. Some differences from other async frameworks
* rust doesn't provide a default async runtime, you need to import one (the most popular is `tokio`)
* rust doesn't have a notion of "background tasks", a task is frozen unless awaited

Async rust revolved around the `Future` trait, which can be awaited, and an `async` scope, which can await.

```rust
fn foo()->impl Future<Output=u32>{
    let ret = async {
        sleep(Duration::from_millis(700)).await;
        42
    };
    ret
}

fn main(){
    let mut rt = tokio::runtime::Runtime::new().unwrap();

    let future = async {
        println!("the answer is: {}", foo().await);
    };
    rt.block_on(future); // the answer is: 42
}
```

we can sugar this with `async fn`

```rust
async fn foo()->u32{
    sleep(Duration::from_millis(700)).await;
    42
}

#[tokio::main]
async fn main(){
    println!("the answer is: {}", foo().await); // the answer is: 42
}
```