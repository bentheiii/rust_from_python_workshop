```rust
fn remove_zeroes<'a>(s: &'a Cow<[usize]>)->Cow<'a, [usize]>{
    let is_zero = |x: &usize| *x == 0usize;
    let is_nz = |x: &usize| *x != 0usize;
    
    if let Some(first_nz_index) = s.iter().position(is_nz){
        if let Some(first_z_after_nz_index) = s.iter().skip(first_nz_index+1).position(is_zero) {
            let first_z_after_nz_index = first_z_after_nz_index + first_nz_index+1;
            if let Some(first_nz_after_z_index) = s.iter().skip(first_z_after_nz_index+1).position(is_nz){
                let first_nz_after_z_index = first_nz_after_z_index + first_z_after_nz_index+1;
                // we need to clone
                let mut ret = s[first_nz_index..first_z_after_nz_index].to_vec();
                for item in &s[first_nz_after_z_index..]{
                    if *item != 0{
                        ret.push(*item);
                    }
                }
                Cow::Owned(ret)
            } else {
                // [0,...,0, NZ, ..., NZ, 0, ..., 0]
                Cow::Borrowed(&s[first_nz_index..first_z_after_nz_index])
            }
        } else {
            // [0,...,0, NZ, ..., NZ]
            Cow::Borrowed(&s[first_nz_index..])
        }
        
    } else {
        // it's all zeroes
        Cow::Owned(vec![])
    }
}
```