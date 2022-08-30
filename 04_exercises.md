# Exercise 1

Create a picker bot. Your bot will receive a list of names, and choose one or more names from them.

expect you input to be a line separated list of names. Optionally, after each name, the user might specify a number that indicates the odds of this name being chosen relative to the others (the default "share" is 1).

finally, the user will enter the special command `!pick [X [with replacement]]`, which will indicate that `X` names should be randomly chosen from the list (or 1, if `X` is not specified). Unless specified with `with replacement`, the name name should not be chosen twice.

example input:
```
Jim
Dwight 2
Micheal 0
Pam
Oscar
!pick 2 with replacement
```

# Exercise 2

Implement a function that accepts an iterator an returns an iterator of all the increasing sub-sequences in the iterator
```
split_to_increasing([[1,6,2,2,8,11,12,-1,0,2,1]]) == [[1, 6], [2, 2, 8, 11, 12], [-1, 0, 2], [1]]
```

# Exercise 3

Implement a function that, given in iterator of ordered elements, returns the length of the [longest increasing subsequence](https://en.wikipedia.org/wiki/Longest_increasing_subsequence) in the iterator.