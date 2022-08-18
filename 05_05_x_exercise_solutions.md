# Exercise 1
```rust
impl<T> LinkedList<T>{
    fn new()->Box<Self>{
        Box::new(Self::Empty)
    }
    
    fn from_iter(it: impl IntoIterator<Item=T>)->Box<Self>{
        let mut ret = Self::new();
        for value in it{
            ret = Box::new(Self::Cons{value, next: ret});
        }
        ret
    }
    
    fn nth(self: &Box<Self>, index: usize)->T where T: Clone{
        match self.as_ref(){
            Self::Empty => panic!("index out of range"),
            Self::Cons{value, next} => {
                if index == 0{
                    value.clone()
                } else {
                    next.nth(index - 1)
                }
            }
        }
    }
}
```

# Exercise 2
```rust
enum Tree<T: Ord>{
    Empty,
    Node {value: T, left: Box<Self>, right: Box<Self>}
}

impl<T: Ord> Tree<T>{
    fn new()->Self{
        Self::Empty
    }
    
    fn add(&mut self, value: T){
        match self{
            Self::Empty => {
                *self = Self::Node{value,
                                   left: Box::new(Self::new()),  
                                   right: Box::new(Self::new())};
            }
            Self::Node{value: mid, left, right} => {
                if *mid == value{
                    return;
                } else if *mid > value{
                    left.add(value);
                } else {
                    right.add(value);
                }
            }
        }
    }
    
    fn has_element(&self, value: T)->bool{
        match self{
            Self::Empty => false,
            Self::Node{value: mid, left, right} => {
                if *mid == value{
                    true
                } else if *mid > value{
                    left.has_element(value)
                } else {
                    right.has_element(value)
                }
            }
        }
    }
}
```