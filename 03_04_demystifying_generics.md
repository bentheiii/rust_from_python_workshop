It would be useful to explain how exactly generics work. Given the following code:

```rust
trait Perimeter{
    fn perimeter(&self)->f64;
}

trait Area{
    fn area(&self)->f64;
}

struct Rectangle(...)
impl Perimeter for Rectangle {...}
impl Area for Rectangle {...}

struct Triangle(...)
impl Perimeter for Triangle {...}
impl Area for Triangle {...}

struct Circle(...)
impl Perimeter for Circle {...}
impl Area for Circle {...}

fn efficiency<T: Perimeter + Area>(shape: &T){
    shape.area()/shape.perimeter()
}

efficiency(Rectangle(...));
efficiency(Triangle(...));
efficiency(Circle(...));
```

rust actually turns it into this:
```rust
fn efficiency_rectangle(shape: &Rectangle){
    shape.area()/shape.perimeter()
}

fn efficiency_triangle(shape: &Triangle){
    shape.area()/shape.perimeter()
}

fn efficiency_circle(shape: &Circle){
    shape.area()/shape.perimeter()
}

efficiency_rectangle(Rectangle(...));
efficiency_triangle(Triangle(...));
efficiency_circle(Circle(...));
```

generics are actually templates for code, that are copy-pasted and used on the fly.

This means that using the same generic function for 10 different types will result in 10 functions existing in you final executable. This might create what is called "template bloat", although usually that's a non-issue.