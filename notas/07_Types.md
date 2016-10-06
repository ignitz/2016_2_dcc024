# Types (Aula 07)

1) What is a type?
* A type is a set of values
* plus an intention:
  - a low-level representation
  - a collection of operations that can be applied to those values

1.1) Do you know any language that does not have types?

2) What are the uses of types?
- Documentation
- Safety
- Efficiency
- Correctness

* We have two kinds of types: primitive and constructed.

3) What is the difference between them?

* Some languages, like Java, define primitive types exactly. Others, like
ML or C, don't.

4) What are the primitive types of Java?
```
byte (1-byte signed)
char (2-byte unsigned)
short (2-byte signed)
int (4-byte signed)
long (8-byte signed)
float (4-byte floating point)
double (8-byte floating point)
```

4.1) What about C?
```
char
unsigned char
short int
unsigned short int
int
unsigned int
long int
unsigned long int
```

4.2) How to discover the maximum integer in C?
```c
#include <stdio.h>
#include <limits.h>
int main() {
  printf("%d\n", INT_MAX);
}
```

**or**

```c
#include<stdio.h>
int main(int argc, char** argv) {
  unsigned int i = ~0U;
  printf("%d\n", i);
  i = i >> 1;
  printf("%d\n", i);
}
```
```
$> a.out
-1
2147483647
```

4.3) What about SML?
```
- Int.maxInt;
val it = SOME 1073741823 : int option
```

**or**

E.g: use "`fun sum a b = a + b  handle Overflow => 0;`" as an example.
```
fun maxInt current inc = maxInt (current + inc) (inc * 2)
    handle Overflow => if inc = 1 then current else maxInt current 1
```

* Constructed types are just sets built from other sets.

5) What is the simplest constructed set? Or, what is the simplest way to
build a set from another set?  
5.1) How to declare enumerations in C?
```c
// enum.c
#include <stdio.h>
enum coin { penny = 1, nickel = 5, dime = 10, quarter = 25 };
int main() {
  printf("%d\n", penny);
  printf("%d\n", penny + 1);
  printf("%d\n", penny + 4 == nickel);
}
```

5.2) What about SML?
```
- datatype day = M | Tu | W | Th | F | Sa | Su;
- fun isWeekend x = (x = Sa orelse x = Su);
```

6) What is another way to build sets?

* Some languages support true tuple types:
```
fun get1 (x : real * real) = #1 x;
- get1 (3.4, 4.3);
val it = 3.4 : real
```

7) How to implement tuples in C?
```c
// complex.c
#include <stdio.h>
struct complex {
  double rp;
  double ip;
};
int main() {
  struct complex c1;
  c1.rp = 0;
  printf("%d\n", c1.rp);
  printf("%d\n", c1.ip);
}
```

* named record types are tuples indexed by names, instead of numeric indices.
SML has those too:
```
type complex = {
  rp:real,
  ip:real
};
fun getip (x : complex) = #ip x;
```

* Types have an internal representation. Some languages hide it from
programmers, others don't.

8) How are tuples arranged in memory in C?  
8.1) How to iterate on the fields of a tuple of integers in C?  
```c
#include <stdio.h>

struct mySt {
  int i1, i2, i3, i4, sentinel;
};

int main(int argc, char** argv) {
  struct mySt s;
  int *p1, *p4;
  s.i1 = 1;
  s.i2 = 2;
  s.i3 = 3;
  s.i4 = 4;

  p1 = ( (int*) &s);
  p4 = &(s.sentinel);
  do {
    printf("field = %d\n", *p1);
    p1++;
  } while (p1 != p4);
}
```
<!-- ** -->

9) Which other sets can we talk about?
- Vectors are a multidimensional cartesian product of the same set.

9.1) Which types do we derive from vectors?
- arrays, lists, strings.

9.2) What is an array?

* indices vary: Java starts from 0, Pascal is more flexible.

10) How does the implementation of vectors change from one language to the
other?
- What are the index values?
- Is array size fixed at compile time (part of static type)?
- Which operations are supported?
- Is redimensioning possible at runtime?
- Are multiple dimensions allowed?
- Is a higher-dimensional array the same as an array of arrays?
- What is the order of elements in memory?
- Is there a separate type for strings (not just array of characters)?
- Is there a separate type for lists?

11) Which other way to build a set are we missing?  
11.1) What is the difference between union and cartesian product?  
11.2) How to represent union in C?  
```c
// element.c
#include <stdio.h>
union element {
  int i;
  float f;
};
int main() {
  union element e;
  e.f = 177689982.02993F;
  printf("Int = %d\n", e.i);
  printf("Float = %f\n", e.f);
  e.f = 0.0F;
  e.i = 177689982;
  printf("Int = %d\n", e.i);
  printf("Float = %f\n", e.f);
}
```

11.3) How are unions implemented in C?

* In ML we represent unions via datatypes:
```
datatype element =
  I of int | F of real;
fun getReal (F x) = x
  | getReal (I x) = real x;
```

12) Which type is missing now?

* Functions map a given set, the domain, into another set, the range.

13) How would be this function below in SML?
```
int f(char a, char b) {
  return a==b;
}

- fun f (a:char, b) = a = b;
```

13.1) Which operations can we perform on functions?
```
fun comp c a b = c (a, b);

fun ci (a:real, b) = not (a > b orelse a < b);

comp ci 2.3 4.5
```

14) When to check the types used in a program?
- At compilation time.
- At execution time.

14.1) Examples of statically typed language?  
14.2) Examples of dynamically typed languages?  
14.3) Examples of a type error in C?  
14.4) Examples of a type error in ML?  

15) How to determine the types of values in a program during compilation?
- Type annotations.
- Naming conventions: e.g: I for integer in Fortran, and S$ for strings
in Basic.
- Type inference: Haskell, ML, Scala, Ocaml, C#3.0

15.1) Can anyone give me examples of type inference in Java?
```
double a; sysout(a*0);
```

16) What is the advantage of dynamically typed languages?  
16.1) Can someone give me an example?
```ruby
// Consider the program below, written in Ruby. It handles big numbers.
// However, the dynamic type of the factorial of a small number is integer,
// and the dynamic type of the factorial of a big number is long.
fact = 1;
(1..100).each do |i|
  fact = fact * i;
end
puts fact
```
**or**
```php
<?php
$c = 1;
if ($c == 1) {
  $var = true;
} else if ($c == 2) {
  $var = 42;
} else if ($c == 3) {
  $var = 3.14;
} else {
  $var = "dcc024";
}
echo "Is int? " . is_int($var) . "<br>";
echo "Is bool? " . is_bool($var) . "<br>";
echo "Is float? " . is_float($var) . "<br>";
echo "Is string? " . is_string($var) . "<br>";
?>
```

See: http://homepages.dcc.ufmg.br/~fpereira/coisas/test.php

17) How a dynamically typed language would be implemented?

* It is not black and white:
- Statically typed languages do some dynamic checks.

18) Examples in Java?
```java
class Pencil {
  public int p;
}
class Battleship {
  public int p;
  public static void main () {
    Object p = new Pencil();
    p.p = 11;
    Battleship b = (Battleship)p; // Check happens here!!!
    System.out.println(p.p + ", " + b.p);
  }
}
```

- Dynamically typed languages use a bit of type inference.

* Some languages allow runtime type tests.
19) Can anyone give me an example?

* There exists languages which are strongly typed, and languages which
are weakly typed.
- A strongly typed language guarantees that a type will be always used
as declared.
20) Can anyone give me an example?
- A weakly typed language allows a value to be used as if it had a type
different from its declaration type.

20.1) Examples?

20.2) Examples of unsafe type usage in C?
```c
union element {
  int i;
  float f;
};
int main() {
  union element e;
  e.f = 17.0;
  printf("%lf\n", e.i);
}
```
**or**
```c
int main() {
  int a[3] = {1, 2, 3};
  printf("%d\n", a[3]);
}
```

21) Can anyone give me an example of unsafe C++ code?

21.1) What is the code below going to print?
```cpp
#include <stdio.h>
#include <string.h>
class Pencil {
  public:
    int p;
};
class Battleship {
  public:
    int b;
};
int main () {
  Pencil *p = new Pencil();
  p->p = 17;
  Battleship *b = new Battleship();
  b->b = 11;
  printf("p->p = %d, b->b = %d\n", p->p, b->b);
  b = (Battleship*)p;
  printf("p->p = %d, b->b = %d\n", p->p, b->b);
}
```

22) Can anyone give me an example of unsafe memory protection in C?
```c
// iterStruct2.c
int main() {
  char* s = "Hello!";
  char* s1 = "Fernando!";
  char* s2 = "Magno!";
  char* s3 = "Quintao!";
  char* s4 = "Pereira!";
  char* c = s;
  int i;
  for (i = 0; i < 41; i++) {
    printf("%c", *c);
    c++;
  }
  printf("\n");
}
```
<!-- ** -->

22.1) What would happen if I had replaced the loop by:
```c
  for (i = 0; i < 41; i++) {
    printf("%c", *c);
    *c = 'a';
    c++;
  }
```
<!-- ** -->

23) When are two types equivalent?
- There are two ways to decide: name equivalence and structural equivalence:

Ex.: SML
```
- type irpair1 = int * real;
type irpair1 = int * real
- type irpair2 = int * real;
type irpair2 = int * real
- fun f(x:irpair1) = #1 x;
val f = fn : irpair1 -> int
- val (y:irpair2) = (3, 3.14);
val y = (3,3.14) : irpair2
- f y;
val it = 3 : int
```

23.1) What about C++?
```cpp
// See also UnsafeCoerc.cpp
#include <stdio.h>
#include <string.h>
class Pencil {
  public:
    int p;
};
class Battleship {
  public:
    int p;
};
int main () {
  Pencil *p = new Pencil();
  Battleship *b = p;
  printf("p->p = %d, b->b = %d\n", p->p, b->p);
}
```

<!-- ** -->
