A program's memory is divided into 4 sections, and the program can alter them in different ways:
![](https://upload.wikimedia.org/wikipedia/commons/thumb/5/50/Program_memory_layout.pdf/page1-234px-Program_memory_layout.pdf.jpg)
* code segment/text
  * This is where the CPU instructions are stored for the program
  * It is only referenced in the CPU's instruction pointer
  * This segment cannot be edited
* data + bss
  * This is where all the global variables and consts are stored
  * The variables here can be changed, but their size, like the segment's size, cannot change.
  * This means that only known-size variables can be placed here. (you can't have a global list in the data segment)
* stack
  * the call stack (or just stack), contains information about the scope the program is currently executing, as well as the scope that called it, and so on...
  * Each "Layer" in the call stack is allocated when a function is entered, and freed when a function is exited. The layer includes things like a function's return value and a pointer to the beginning of the previous layer.
  * Many times the call stack will also include some constant-size variables the function is sure to use.
  * Each layer in the stack is always allocated as a whole or freed as a whole. All stack variables are deleted when the associated scope is exited.
* heap
  * The heap is where we can manually manage memory.
  * The main difference between the heap and the stack is that we have total control of when the data we allocate is deleted.

With this knowledge: what happens in memory when this is run?

```python
n = 10
m = 20
if random() > 0.5:
    n = [1,2,3]
print(n*m)
```