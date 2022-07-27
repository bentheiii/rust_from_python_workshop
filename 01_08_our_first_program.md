enough talk, let's install rust!
* more details about everything can be found in the [rust book](https://doc.rust-lang.org/book/title-page.html).
* You should also check out [rustlings](https://github.com/rust-lang/rustlings) for more exercises.
* first we need to install rustup. rustup manages multiple rust installations. (it's like pyenv but official).
* in linux: `$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh`
* create a new project with `cargo new hello_rust`
* You will see a new directory for your project, and in it a file called `main.rs`
* create a file called `main.rs` with the following text:
  ```rust
  fn main() {
    println!("Hello, world!");
  }
  ```
  output:
  ```
  Hello, world!
  ```
* build and run with `cargo run`