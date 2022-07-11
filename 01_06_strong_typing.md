* strong vs weak- typed languages is a loose definition. It generally refers to how many castings can occur implicitly.

* Javascript for example is very weakly-typed
  ```javascript
  >>> [16, 17] + 18
  '16,1718'
  ```
* languages like Java and C# are also rather weakly-typed
  ```Java
  class Main {
    public static void main(String[] args) {
        System.out.println("2 + 3 = " + 5); 
    }
  }
  ```
* python is pretty strong in this regard, but it also has some implicit castings
  ```python
  >>> 6 + 3.6
  9.6
  >>> x = [1,2]
  >>> y = {3, 4}
  >>> x.extend(y)
  >>> x
  <either [1,2,3,4] or [1,2,4,3]>
  >>> a = [1,2]
  >>> b = {3: 'three', 4: 'four'}
  >>> a.extend(b)
  >>> a
  <either [1,2,3,4] or [1,2,4,3]>
  ```
* rust has absolutely no implicit casting, and no overloading*
  ```rust
  fn foo() -> f64 {
    return 12; // this will fail!
  }
  ```