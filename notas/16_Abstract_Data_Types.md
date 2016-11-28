# Abstract Data Types

---------
Has anyone heard of Barbara Liskov?

---------
What is an abstract data type?

---------
Can you work out a set datatype in C? Can you implement it as a bit-set?

```c
// set.h
#ifndef SET_H
#define SET_H
#define INT_BITS 32
typedef struct {
  unsigned* vector;
  unsigned size;
} set;
void new(set*, unsigned);
void add(set*, unsigned);
void del(set*, unsigned);
int contains(set*, unsigned);
#endif

// set.c
#include <stdlib.h>
#include "set.h"
void new(set* s, unsigned capacity) {
  s->vector = (unsigned*)malloc((1 + capacity / INT_BITS) * sizeof(unsigned));
  s->size = capacity;
}
void add(set* s, unsigned e) {
  unsigned index = e / INT_BITS;
  unsigned offset = e % INT_BITS;
  unsigned bit = 1 << offset;
  s->vector[index] |= bit;
}
void del(set* s, unsigned e) {
  unsigned index = e / INT_BITS;
  unsigned offset = e % INT_BITS;
  unsigned bit = 1 << offset;
  s->vector[index] &= ~bit;
}
int contains(set* s, unsigned e) {
  unsigned index = e / INT_BITS;
  unsigned offset = e % INT_BITS;
  unsigned bit = 1 << offset;
  return s->vector[index] & bit;
}
```

---------
What is the role of the header file?

---------
What is the role of the c file?

---------
Do you have access to the internal representation of a set?

```c
#include <stdio.h>
#include "set.h"
int main() {
  set s;
  new(&s, 60);
  add(&s, 2);
  add(&s, 3);
  add(&s, 5);
  printf("Contains %d? %d\n", 1, contains(&s, 1));
  printf("Contains %d? %d\n", 2, contains(&s, 2));
  printf("Contains %d? %d\n", 3, contains(&s, 3));
  printf("Contains %d? %d\n", 4, contains(&s, 4));
  printf("Contains %d? %d\n", 5, contains(&s, 5));
  del(&s, 5);
  printf("Contains %d? %d\n", 5, contains(&s, 5));
  printf("Size = %d\n", s.size);
  s.vector[0] = 1431655765;
  printf("Contains %d? %d\n", 2, contains(&s, 2));
  printf("Contains %d? %d\n", 3, contains(&s, 3));
  printf("Contains %d? %d\n", 4, contains(&s, 4));
}
```

---------
What are the disadvantages of having access to the internal data
representation?

---------
How to find the length of a chain of characters in C?

---------
What would happen if the C committee decided to change the implementation
of chains of characters to use a '\1' instead of a '\0' at the end of the
array?

---------
How to describe the set datatype in SML?

```
signature SET =
sig
  type set
  val new : set
  val add : set -> int -> set
  val contains : set -> int -> bool
  val remove : set -> int -> set
end ;
```

---------
And how to provide an implementation to it?
```
structure FunSet :> SET =
struct
  type set = int -> bool
  val new = fn x => false
  fun add f i = fn x => if x = i then true else f x
  fun contains f i = f i
  fun remove f i = fn x => if x = i then false else f x
end ;
```

---------
How can it be used?

```
structure S = FunSet ;
val s = S.new ;
val s = S.add s 3 ;
val contains1 = S.contains s 1 ;
val contains3 = S.contains s 3 ;
val s = S.remove s 3 ;
val contains3 = S.contains s 3 ;
```

---------
Is there a way to know the internal representation of this datatype?

---------
How to change an operation of the set datatype implemented in SML?

---------
How to code the same datatype in Python?

```python
INT_BITS = 32
def getIndex(element):
  index = element / INT_BITS
  offset = element % INT_BITS
  bit = 1 << offset
  return (index, bit)
class Set:
  def __init__(self, capacity):
    self.vector = range(1 + capacity / INT_BITS)
    for i in range(len(self.vector)):
      self.vector[i] = 0
  def add(self, element):
    (index, bit) = getIndex(element)
    self.vector[index] |= bit
  def delete(self, element):
    (index, bit) = getIndex(element)
    self.vector[index] &= ~bit
  def contains(self, element):
    (index, bit) = getIndex(element)
    return (self.vector[index] & bit) > 0
```

---------
What is exactly a class?
Which other languages have this concept?

---------
How classes are used?

```python
>>> s = Set(60)
>>> s.add(4)
>>> print "Contains 4? ", s.contains(4)
>>> print "Contains 5? ", s.contains(5)
>>> s.delete(4)
>>> print "Contains 4? ", s.contains(4)
```

* Notice that this o.m() syntax is just syntactic sugar:
```python
>>> Set.add(s, 1)
>>> Set.contains(s, 1)
```

* And classes are "almost" just tables:
```python
>>> a = {}   
>>> a['contains'] = Set.contains
>>> b = Set(20)
>>> a['contains'](b, 3)
False
>>> a['add'] = Set.add
>>> a['add'](b, 3)
>>> a['contains'](b, 3)
True
```

---------
What is an object?

---------
What are the differences between objects and classes?

---------
ALERT: SPOILER of next class => What is object oriented programming?

---------
Is there data encapsulation in Python?

```py
>>> print s.vector
[0, 0]
>>> s.vector[0] = 2
>>> s.contains(1)
```

---------
In terms of implementation, what are classes in Python?

---------
What would happen if we insert a number out of range in our Python implementation of set?
```py
>>> s = Set(15)
>>> s.add(1)
>>> s.add(17)
>>> s.add(32)
```

---------
I would like to provide an error handling mechanism in this implementation
of set: if we try to insert a number out of range, an exception should be
fired.

```py
from Set import INT_BITS, getIndex, Set
def errorAdd(self, element):
  if (element > self.capacity):
    raise IndexError(str(element) + " is out of range.")
  else:
    (index, bit) = getIndex(element)
    self.vector[index] |= bit
    print element, "added successfully!"

Set.add = errorAdd
s = Set(60)
s.add(59)
s.add(60)
s.add(61)
```

---------
Is there another way to extend the set datatype in Python?

```py
from Set import INT_BITS, getIndex, Set
class ErrorSet(Set):
  def checkIndex(self, element):
    if (element > self.capacity):
      raise IndexError(str(element) + " is out of range.")
  def add(self, element):
    self.checkIndex(element)
    Set.add(self, element)
    print element, "successfully added."
  def delete(self, element):
    self.checkIndex(element)
    Set.delete(self, element)
    print element, "successfully removed."
  def contains(self, element):
    if element > self.capacity:
      return false
    else:
      return Set.contains(self, element)

s = ErrorSet(60)
s.add(59)
s.add(60)
s.add(61)
```

---------
Is there any difference between the interface of Set and ErrorSet?

---------
Code a function `fill(set, b, e, s)` that adds all the number in `range(b, e, s)` to set.

```py
def fill(set, b, e, s):
  for i in range(b, e, s):
    set.add(i)
```

---------
Is it a valid call?
```py
s0 = Set(15)
fill(s0, 10, 20, 3)
```

---------
Has anyone ever heard of the Liskov substitution principle?  
"If S is a subtype of T, then S can be used in any situation in which T is expected."

---------
Explain this apparent paradox:  
"The subtype has more properties than the supertype, but the supertype has more elements than the subtype."

---------
Do you think this is a valid call?
```py
class Num:
  def __init__(self, num):
    self.n = num
  def add(self, num):
    self.n += num
  def __str__(self):
    return str(self.n)

n = Num(3)
print n
fill(n, 1, 10, 1)
print n
```

---------
Python uses 'duck-typing', i.e., "If it walks like a duck, swims like a
duck and squeaks like a duck, then it is a duck". How does this explains the
behavior of Num and fill?

---------
Do you think the approach for object orientation would change on a
statically typed language, like Java?

---------
How to describe the set ADT in Java?
```java
public interface Set<E> {
  void add(E element);
  void del(E element);
  boolean contains(E element);
}
```

---------
How to provide an implementation to this ADT?
```java
public class BitSet implements Set<Integer> {
  public final int INT_BITS = 32;
  private int vector[];
  private int capacity;
  public BitSet(int capacity) {
    vector = new int[1 + capacity / INT_BITS];
    this.capacity = capacity;
  }
  public void add(Integer e) {
    int index = e / INT_BITS;
    int offset = e % INT_BITS;
    int bit = 1 << offset;
    vector[index] |= bit;
  }
  public void del(Integer e) {
    int index = e / INT_BITS;
    int offset = e % INT_BITS;
    int bit = 1 << offset;
    vector[index] &= ~bit;
  }
  public boolean contains(Integer e) {
    int index = e / INT_BITS;
    int offset = e % INT_BITS;
    int bit = 1 << offset;
    return (vector[index] & bit) > 0;
  }
  public static void main(String args[]) {
    Set<Integer> s = new BitSet(15);
    s.add(15);
    System.out.println("Contains 15? " + s.contains(15));
    s.add(16);
    System.out.println("Contains 16? " + s.contains(16));
    s.add(31);
    System.out.println("Contains 31? " + s.contains(31));
    s.add(32);
    System.out.println("Contains 32? " + s.contains(32));
  }
}
```

---------
What are the main differences between this program and the Python program?

---------
What is the meaning of public and private in the program above?
What would happen if I tried `s.capacity = 0`?

Is there any way to read the value of `s.capacity` directly?
```java
try {
  Field f = s.getClass().getDeclaredField("capacity");
  f.setAccessible(true);
  int cap = (int)f.getInt(s);
  System.out.println("Value of field = " + cap);
} catch (Exception e) {
  e.printStackTrace();
}
```

---------
What is the meaning of the declaration `Set<Integer>`?

---------
How to implement a version that only inserts elements up to the set's
capacity?

```java
public class BoundedBitSet extends BitSet {
  public BoundedBitSet(int capacity) { super(capacity); }
  private boolean checkIndex(int e) {
    int index = e / INT_BITS;
    return index < 1 + capacity / INT_BITS;
  }
  public void add(Integer e) {
    if (checkIndex(e)) {
      super.add(e);
    } else {
      System.err.println("Attempt to insert " + e + " in set of capacity " +
          capacity);
    }
  }
  public void del(Integer e) {
    if (checkIndex(e)) {
      super.del(e);
    } else {
      System.err.println("Attempt to remove " + e + " from set of capacity " +
          capacity);
    }
  }
  public boolean contains(Integer e) {
    if (checkIndex(e)) {
      return super.contains(e);
    } else {
      System.err.println("Attempt to access " + e + " in set of capacity " +
          capacity);
      return false;
    }
  }
}
```

---------
Can you implement the same fill function as in the Python example?
```java
public static void fill(Set<Integer> set, int b, int e, int s) {
  for (int i = b; i < e; i += s) {
    set.add(i);
  }
}
```

---------
Is it possible to pass anything that is not instance of Set<Integer> to fill? Like in the Python example?

---------
Can you come up with a quick summary about these four languages tools to implement ADTs?

```
           Encapsulation       Extension
C                     No              No
SML                  Yes              No
Python                No             Yes
Java                 Yes             Yes
```
