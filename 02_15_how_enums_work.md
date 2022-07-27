* It's important to demystify `enums`. In order to understand enums, we need to understand `union`s. A `union` is like an `enum` except we don't know which state the value is in.
  ```rust
  union A{
    f1: f32, // 4 bytes
    f2: [u8; 3], // 3 bytes
    f3: u32 // 4 bytes
  }
  let u = A { f1: 3.1415 };
  // u is of size 4, it's current state is
  // [86, 14, 73, 64]
  assert!(unsafe{u.f2} == [86, 14, 73]);
  assert!(unsafe{u.f3} == 1_078_529_622);
  ```
* `enum`s are just union with an extra marker at the start (called a "tag") that tells us which state the value is in.
* thus, `Some(12)` is stored as the tuple `(1, 12)`, where the `1` indicates that the `enum` is a `Some`, and the `12` is the actual value.
* `None` is stores as the Tuple `(0, ?)` where the `0` indicates that the `enum` is a `None` and the second value doesn't matter.
  ```
  if let None = my_optional{ // this comparison checks only the tag
    ...
  }
  ```
* In most cases, an `enum` will require as much space as its largest variant, plus the size of the tag.