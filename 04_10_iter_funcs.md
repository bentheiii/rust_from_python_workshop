the `std::iter` module has some nice functions to create iterators.
* `iter::empty` for an empty iterator
* `iter::once` to create an iterator with only one element
* `iter::repeat` to create an infinite iterator with one element
  ```rust
  assert_eq!(iter::repeat(5).take(3).collect::<Vec<_>>(), vec![5,5,5])
  ```
* `iter::repeat_with` to create an infinite iterator with a callable
  ```rust
  let mut ctr = 0;
  assert_eq!(iter::repeat_with(|| {ctr += 1; ctr}).take(5).collect::<Vec<_>>(), vec![1,2,3,4,5])
  ```
* `iter::from_fn` to create an iterator from a callable (this is the rust equivalent of a yield function)
  ```rust
  fn parts(start: usize, base: usize)->impl Iterator<Item=usize>{
    let mut current = start;
    iter::from_fn(move ||{
        if current > 0{
            let ret = current;
            current /= base;
            Some(current%base)
        } else {
            None
        }
    })
  }

  assert_eq!(parts(15687, 10).collect::<Vec<_>>(), [7,8,6,5,1]);
  ```