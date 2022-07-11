* we've already seen the `for` loop and `while` loop, which behave very similar to how they do in python, but there is a third kind of loop: the `loop` loop.
* `loop` is essentially equivalent to `while true`, and should be used instead of it. The difference between then is that `loop` is provably only exitable via the `break` command, and therefore has a neat additional feature.

  ```rust
  {
    let x = loop {
        let num: f64 = rand::random();
        if num > 0.5 {
            break num;
        }
    };
    println!("{x:?}");
  }
  ```

# Exercise 1
The collatz sequence for a given number is a generating dequence with the following rules depending on the previous value `n`:
* if `n` is even, the next item is `n/2`
* if `n` is odd, the next item is `n*3 + 1`
* if `n` is 1, the sequence ends

So for example, the collatz sequence for the number `6` is: `6, 3, 10, 5, 16, 8, 4, 2, 1`. It is conjectured that the collatz sequence of any positive number is finite.

implement a function that calculates the length of the collatz sequence of a given number.

```
collatz_length(6) == 9
collatz_length(1) == 1
collatz_length(2) == 2
```

# Exercise 2
implement the above function in one line.