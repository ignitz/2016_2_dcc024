# Memory Management

Which "program things" need memory storage?

------------------------------
Where are these "things" stored?

------------------------------
There are three ways to manage memory. What are they?
- Static memory
- Stack memory
- Heap memory

------------------------------
Which type of storage will be performed for each variable below?

```cpp
#include <iostream>

int global_var = 7 ;
const int global_const = 9 ;
const int global_const2 = std::rand() ;

void foo() {
  int auto_var = 9 ;
  int auto_const = 9 ;
  const int auto_const2 = std::rand() ;
  static int static_var = 7 ;
  static const int static_const = 9 ;
  static const int static_const2 = std::rand() ;
  int* ptr = new int(100) ;
}
```

Who takes care of organizing this memory: the architecture, the
compiler or the SO?

------------------------------
What is static storage?
- Advantages?
- Disadvantages?

------------------------------
As an example, what does this program do?
```java
import java.util.List;
import java.util.LinkedList;

public class Pop {
  private static int popNum = 0;
  private int thisPop;
  public Pop() {
    thisPop = popNum++;
  }
  @Override
  public String toString() {
    return "Pop " + thisPop;
  }
  public static void main(String args[]) {
    List<Pop> pops = new LinkedList<Pop>();
    for (int i = 0; i < 10; i++) {
      pops.add(new Pop());
    }
    for (Pop p : pops) {
      System.out.println(p);
    }
  }
}
```

------------------------------
And what will be printed by this program below?

What is the scope of a static variable declared inside a function?
```cpp
#include <stdio.h>
int counter1() {
  static store = 0;
  store++;
  printf("Counter 1 = %d\n", store);
  return store;
}
int counter2() {
  static store = 0;
  store++;
  printf("Counter 2 = %d\n", store);
  return store;
}
int main() {
  int i;
  char* s = "Counter = ";
  for (i = 0; i < 5; i++) {
    if (i % 2) {
      counter1();
    } else {
      counter2();
    }
  }
}
```

------------------------------
When I call a function, where are the variables in the function
stored?

------------------------------
How is the stack organized? Does it grow up, or does it grow down?

In the program below, `sum.c`, which address will be bigger, `&aux_1`, `&aux_2`
or `&aux_3`?
```cpp
#include <stdio.h>
int sum(int a, int b) {
  int aux_1 = a;
  int aux_2 = b;
  int aux_3;
  printf("&aux_1 = %lu\n", &aux_1);
  printf("&aux_2 = %lu\n", &aux_2);
  printf("&aux_3 = %lu\n", &aux_3);
  aux_3 = aux_1 + aux_2;
  return aux_3;
}
int main() {
  printf("Sum = %d\n", sum(2, 3));
}
```

> The activation record of sum has at least six words, two for the
parameters, three for the locals, and one for the return address.

- Example: `slides 12/13`, where we allocate `m = 3`, `m = 2`, and `m = 1` in a stack
with eigth cells.

Is the size of the activation record always fixed?
```cpp
int getLength(char* s1, char* s2) {
  char str[strlen(s1) + strlen(s2) + 1];
  strcpy(str, s1);
  strcpy(str, s2);
  return sizeof(str);
}
```

-------------------
If I had to implement this memory manager in Java, how would that be,
considering that its interface would have two methods:
```cpp
public int push(int requestSize)
public void pop()
```

---------------------
Why is stack allocation so simple to implement?

---------------------
Some data require out-of-order allocation. Which data is this?

---------------------
How to implement a memory management system for dynamically allocated data?

* A free block has the structure: `size; Next Free Block; empty area (size - 2)`
* A full block has the structure: `size; occupied area (size - 1)`

```cpp
@ allocate(int size) {
  @ aux = FL;
  @ lag = NULL;
  while (aux != NULL && * aux < size + 1) {
    lag = aux;
    aux = * aux + 1;
  }
  if (aux == NULL) {
    throw new OutOfMemoryError();
  }
  @ nextFree = aux + size + 1;
  * nextFree = * aux - size - 1;
  * (nextFree + 1) = * (aux + 1);
  if (lag == NULL) {
    FL = nextFree;
  } else {
    * (lag + 1) = nextFree;
  }
  * aux = size + 1;
  return aux + 1; // the size of the block cannot be overwritten
}

deallocate(@p) {
  @aux = p - 1;
  * p = FL;
  FL = aux;
}
```

* Example, slides 20-24

---------------------
How would be the sequence below?

```cpp
p1 = m.allocate(4);
p2 = m.allocate(2);
m.deallocate(p1);
p3 = m.allocate(1);
```

---------------------
Which problem do you see in this kind of memory management?

What about:
```cpp
p1 = m.allocate(4);
p2 = m.allocate(4);
m.deallocate(p1);
m.deallocate(p2);
p3 = m.allocate(7);
```

---------------------
How to solve this problem?

---------------------
Small blocks tend to be allocated and deallocated much more often.  
How to use this knowledge to speed up memory management?

---------------------
Even with quick lists and coalescing, which other problem we see in
this type of management?
- Ex.:
```cpp
p1 = m.allocate(4);
p2 = m.allocate(1);
m.deallocate(p1);
p3 = m.allocate(5);
```

* Heap management deals with three main issues:
  - Placement: where to allocate a block
  - Splitting: when and how to split large blocks
  - Coalescing: when and how to recombine

---------------------
Where to allocate a block?
- first fit, best fit, next fit
- Lists, Trees, Hashtables, etc

---------------------
When and how to split a large block?
- We are splitting to deliver the requested size.
- Sometimes it is better to split in a way to create blocks of well-known sizes.
- Ex.: region management in web servers.

---------------------
When and how to recombine adjacent free blocks?
- No coalescing
- Eager coalescing
- Delayed coalescing

---------------------
Which are common errors in memory management?

- Memory leak:
---------------------
What is the problem here?
```cpp
#include <stdio.h>
#include <stdlib.h>
void problem() {
  int* i = (int*) malloc (sizeof(int));
  * i = 3;
  printf("%d\n", * i);
}
int main() {
  problem();
}
```

- Dangling pointer:
---------------------
21) What will be printed?
```cpp
#include <stdio.h>
#include <stdlib.h>
void dangling() {
  int* i = (int*) malloc (sizeof(int));
  int* j;
  * i = 3;
  free(i);
  j = (int*) malloc (sizeof(int));
  * j = 8;
  printf("%d\n", * i);
}
int main() {
  dangling();
}
```

---------------------
We can use valgrind to find out these errors to us:
```
$> gcc -g Leak.c
$> valgrind -v ./a.out
```

- or -

```
$> gcc -g Dangling.c
$> valgrind -v ./a.out
```

---------------------
How to read the error message in this case?

---------------------
How are these problems solved by modern programming languages?

Which languages use a garbage collector?

---------------------
If I had to ask you to implement a garbage collector, how would you
do it?

* Mark and sweep: find the live heap links and mark those blocks that are
reachable. Then make a pass over the heap and return unmarked free blocks
to the free pool.

* Copying collection: memory is divided in two; only one half is used at
a time. When the used half is full, copy used blocks to the other location,
and erase the old one.

---------------------
Advantages?

What is the main complication in this scheme?
Pointers must be changed.

---------------------
Why is it so difficult to implement in C?

- What if I change a variable that is not a current heap link?
```cpp
int main(int argc, char**  argv) {
  int* p1 = (int*) malloc (sizeof(int));
  char a[80];
  printf("Address = %d\n", p1);
  scanf("%s", a);
  {
    int* p2 = atoi(a);
    int* p3;
    int i = p2;
    p1[0] = 3;
    printf("Address = %d\n", p2);
    printf("Address = %d\n", * p2);
    printf("Address = %d\n", i);
    p3 = i;
    * p3 = 4;
    printf("Address = %d\n", * p2);
  }
}
```

---------------------
Such a garbage collector would crash with the old '`xor list`'. Do you know
how this list works?

```
A         B         C         D         E  ...
B   <->  A^C  <->  B^D  <->  C^E  <->

visit(first) {                 |    visit(node, prev) {
  process(first)               |      process(node)
  visit(first.next, &first)    |      visit(node.next ^ prev, &node)
}                              |    }
```

* Reference counting: each block has a counter of links that point to it.
This counter is incremented when a heap link is copied, decremented when
the link is discarded. When the counter goes to zero, the block is
freed.

---------------------
Advantages?
- naturally incremental, with no big pause.

What is the main problem with this scheme?

---------------------
Which improvements can we think for garbage collection?
- Generational collectors: divide block into generations, according to age.
- Incremental: does GC a little at a time.

---------------------
When should I choose a language without garbage collection? When
should I go for GC?
