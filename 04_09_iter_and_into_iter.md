For most data structures, there are two ways to create an iterator:
* `iter`: creates an iterator that references the original object `&Vec<T> -> impl Iterator<Item = &T>`
* `into_iter`: consumes the object and returns an iterator `Vec<T> -> impl Iterator<Item = T>`
