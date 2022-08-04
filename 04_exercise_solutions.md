# Exercise 2

```rust
fn longest_increasing_subsequence<I: Iterator>(iter: I)->Vec<I::Item> 
where I::Item : Copy + PartialOrd
{
    let mut increasing_lists = vec![];
    for item in iter {
        let split_ind = increasing_lists
            .partition_point(|lst: &Vec<I::Item>| lst.last().unwrap() < &item);
        let new_lst = split_ind.checked_sub(1)
            .map(|i| &increasing_lists[i])
            .map_or_else(
                    || vec![item],
                    |prev| prev.iter().cloned().chain(once(item)).collect()
            );
        if split_ind == increasing_lists.len(){
            // create a new list
            increasing_lists.push(new_lst);
        } else {
            increasing_lists[split_ind] = new_lst;
        }
    }
    increasing_lists.last().cloned().unwrap_or_default()
}
```