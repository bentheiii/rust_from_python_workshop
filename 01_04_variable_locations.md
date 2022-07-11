* the problem of needing to manually free memory is the bane of low-level programming.
* In rust, every variable has an explicit lifetime decided at compile time, and a set of powerful ownership rules to enforce correctness.
* It's actually non-trivial to put a variable in the heap in rust, aside for common containers like vectors and dictionaries.