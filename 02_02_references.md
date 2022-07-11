* References are a variable type that address an object, without actually holding the data.

    ```rust
    let x = 3;
    let ref_to_x: &i32 = &x;  // no copying takes place
    println!("{ref_to_x:?}");
    ```

* References can also be mutable, that allows us to mutate the value

    ```rust
    let mut x = 3;
    let ref_to_x = &mut x;
    *ref_to_x = 10;
    println!("{x:?}");
    ```

* in most cases, you can shortcut the deref (`*`)

    ```rust
    let mut x = vec![1,2,3,4,5];
    let ref_to_x = &mut x;
    ref_to_x.push(6);
    println!("{x:?}");
    ```

* references are especially useful in functions, where we want to accept values without copying them
    
    ```rust
    fn is_number(s: &String){
        ...
    }
    ```

# Exercise 1
Implement a function that accepts an array 3 numbers, the function will return whether the numbers can represents the sides of a triangle.

`3 numbers can be the sides of some triangle if the sum of the two lower numbers is larger than the larger number`

# Excercise 2
Implement the above function in one line.