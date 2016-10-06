# Data Types (Aula 12)

1) What are data-structures?  
1.1) Which data-structures have we seen in ML?  
1.2) How to define new data-structures?

1) What are primitive types?
* A secret: the boolean type is not really a primitive type:
```
datatype bool = true | false;
```

Data types are union types, where each element of the union has a tag.

0) Unions in C do not have tags. What is the disadvantage of not having a tag?

Enumerations
1) How do I define enumerations in C?
```c
#include <stdio.h>
enum Dia {Mon = 3, Tue, Wed, Thu, Fri, Sat, Sun};
int main () {
  printf("%d, %d, %d, %d, %d, %d, %d\n",
    Mon, Tue, Wed, Thu, Fri, Sat, Sun);
  printf("%d\n", Mon + Tue);
}
```

Enumerations in C are very transparent to the developer... but in ML it is
different:

We create enumerations in ML with datatypes:
```
datatype day = Mon | Tue | Wed | Thu | Fri | Sat | Sun;
fun isWeekDay x = not (x = Sat orelse x = Sun);
isWeekDay Mon;
isWeekDay Sat;
```

2) What is the value of Mon + Tue?
```
datatype flip = Heads | Tails;
fun isHeads x = (x = Heads);
```

2.1) Howt to implement isHeads using pattern matching?
```
fun isHeads Heads = true
  | isHeads Tails = false ;

isHeads Tails;
```

3) What is the value of isHeads Mon;?

4) How to modify the isWeekDay function to use patterns?
```
fun isWeekDay Sat = false
  | isWeekDay Sun = false
  | isWeekDay _ = true;
```

Data constructors
- The tags of a datatype are data constructors: they take values, and
  produce elements of the datatype.

5) How to determine a datatype that includes the integers, plus two elements:
minus infinite, and plus infinite?
```
datatype exint = Value of int | PlusInf | MinusInf;
```

Value is a data constructor that takes a parameter of **type int**, and
returns an element of **type exint**.

5.1) Could someone give me examples of elements that are in exint?

5.2) What is the difference between:
```
- Value;
- fun conv x = Value x?
```

6) What would happen: `val x = Value 5; x + x;`
6.1) How can we get the int back from a Value?
```
- val (Value y) = x; y + y;
```

Notice that Value is not a simple function. Simple functions cannot be
pattern matched like this.

6.2) Write a function that converts exint to int, whenever possible:
```
fun e2i (Value x) = x;
```

7) Write a function that converts exint to string:
```
val s = fn x => case x of
    PlusInf => "infinity"
  | MinusInf => "-infinity"
  | Value y => Int.toString y;
```

8) Write a function that returns the square power of exint. How to handle
overflow?

* Pattern matching is also used for exception handling:

```
fun square PlusInf = PlusInf
  | square MinusInf = PlusInf
  | square (Value x) = Value (x * x)
      handle Overflow => PlusInf;
```

8.1) What would happen?
```
- square (Value 4);
val it = Value 16 : exint
- square (Value 99999);
val it = PlusInf : exint
```

8.2) How to find out the largest integer in SML? Could you write a function
for that?
```
fun fi x acc = fi (x + acc) (2 * acc)
      handle Overflow => if acc = 1 then x else fi x 1

val largestInt = fi 1 1;
```

8.3) Can you use a simple case?
```
val square = fn x => case x of PlusInf => PlusInf
                             | MinusInf => PlusInf
                             | Value n => Value (n * n)
                                  handle Overflow => PlusInf;
```

Type constructor:
- The name of a datatype is a type constructor: it takes one or more types
  and produces another type.

Type constructors can also have parameters:
```
datatype 'a option = NONE | SOME of 'a;

SOME 4;
SOME 1.2;
SOME "cat";
```

9) What is a good use for '**option**'?
9.1) Code a function that divides two integers, and returns **NONE** when
the denominator is **zero**:
```
fun optdiv a b =
  if b = 0 then NONE else SOME (a div b);
9.2) Do the same using pattern matching:
fun optdiv a 0 = NONE
  | optdiv a b = SOME (a div b);
```

Contrast it with python, in which the type system does not get into the way:
```
>>> def div(a, b):
...   if b == 0:
...     return "oops!"
...   else:
...     return a / b;
...
>>> div(4.0, 3)
1.3333333333333333
>>> div(4.0, 0)
'oops!'
```

10) Code a type bunch, that is either one single element, or a list of
those elements.
```
datatype 'x bunch =
    One of 'x
  | Group of 'x list;
```

10.1) What is the type of One 1.0;

10.2) What is the type of `Group [true, false]`;

11) Code a function size, that reports the size of an element of type
bunch:
```
fun size (One _) = 1
  | size (Group x) = length x;

size (One 1.0);
size (Group [true, false]);
```

12) What is the type of this function:
```
fun sum (One x) = x
  | sum (Group xlist) = foldr op + 0 xlist;
```

13) How could you implement something similar to bunch in C?
```c
#include <stdio.h>
#include <stdlib.h>
enum bunch_tab {ONE, GROUP};
typedef union {               typedef struct {
  int one;                      bunch_element_type bunch_element;
  int* group;                   unsigned tag;
} bunch_element_type;         } bunch;

bunch* createOne(int i) {
  bunch* b = (bunch*)malloc(sizeof(bunch));
  b->tag = ONE;
  b->bunch_element.one = i;
  return b;
}

void printBunch(bunch* b) {
  switch(b->tag) {
    case ONE:
      printf("%d\n", b->bunch_element.one);
      break;
    case GROUP:
      {
        int i = 0;
        while (b->bunch_element.group[i] != 0) {
          printf("%8d", b->bunch_element.group[i]);
          i++;
        }
      }
  }
}

int main(int argc, char** argv) {
  while (argc > 1) {
    int i;
    argc--;
    i = atoi(argv[argc]);
    bunch* b1 = createOne(i);
    printBunch(b1);
  }
}
```

14) What are the advantages of the ML approach over the C approach?
- Type safety: we could access group in a bunch whose tag is ONE.
- Case coverage. the type system verifies that all the cases are propertly
handled.
- Dead patterns: some might be subsummed by previous patterns, and would never
be reached.

15) Why we cannot have:
```
datatype int option = NONE | SOME of int;?
```

15.1) And how to code this type?
```
- datatype option = NONE | SOME of int
```

15.2) What is 'int in the declaration below?
```
datatype 'int option = NONE | SOME of 'int
```

15) What is a recursive datatype?
```
datatype intlist = INTNIL | INTCONS of int * intlist;
```
Example:
```
INTNIL;
INTCONS (1, INTNIL);
INTCONS (1, INTCONS (2, INTNIL));
```

16) Write a function that gives the length of an intlist:
```
fun intlistLength INTNIL = 0
  | intlistLength (INTCONS(_,tail)) =
        1 + (intListLength tail);
```

17) How can I make this list more generic?
```
datatype 'element mylist = NIL | CONS of 'element * 'element mylist;

CONS;
CONS(1.0, NIL);
CONS(1, CONS(2, NIL));
```

18) How to write a function that adds all the elements of a mylist?
```
fun addup NIL = 0
  | addup (CONS(head,tail)) = head + addup tail;
```

19) How to do this with foldr?
```
fun myfoldr f c NIL = c
  | myfoldr f c (CONS(a,b)) = f(a, myfoldr f c b);
```

E.g:
```
myfoldr f c (a1, a2, a3, a4) = f (a1, f (a2, f(a3, f(a4, c))))

myfoldr (op +) 0 (CONS(1, CONS(2,NIL)));
```

19.a) Would you define the equivalent myfoldl?
```
fun mfoldl f c NIL = c
  | mfoldl f c (CONS(a,b)) = mfoldl f (f(a,c)) b;

mfoldl f c (a1, a2, a3, a4) = f(a4, f(a3, (f(a2, f(a1, c)))))
```

We can even make CONS into an operator, which will be infix:
```
infixr 5 CONS;
1 CONS 2 CONS NIL;
mfoldl (op +) 0 (1 CONS 2 CONS 3 CONS 4 CONS NIL);
```

19.b) What is printed when I type: CONS;?

19.c) What is printed when I type: (op CONS);?

Even the native lists of SML are defined in this way, e.g.:
```
- datatype 'e mylist = nil | :: of 'e * 'e mylist;
- infixr 5 :: ;
```

20) How do I define a binary tree?
```
datatype 'data tree =
   Empty |
   Node of 'data tree * 'data * 'data tree;
```

Example:
```
val treeEmpty = Empty;
val tree2 = Node(Empty,2,Empty);
```
```
(*
 * Implement a function that converts a list to a tree:
 *)
fun toTree1 nil = Empty
  | toTree1 (h::t) = Node (toTree1 t, h, Empty)
```
```
(*
 * Implement a function that converts a list to a balanced tree:
 *)
fun toTree2 nil = Empty
  | toTree2 (h::t) =
    let
      fun split nil = (nil, nil)
        | split [e] = ([e], nil)
        | split (a::b::t) =
          let
            val (l1,l2) = split t
          in
            (a::l1, b::l2)
          end
      val (l1, l2) = split t
    in
      Node (toTree2 l1, h, toTree2 l2)
    end
```
```
(*
 * Implement a function that increments all the elements in the int tree:
 *)
fun incall Empty = Empty
  | incall (Node(x,y,z)) = Node(incall x, y+1, incall z);
```
```
(*
 * Implement a function that sums all the elements of a tree:
 *)
fun sumall Empty = 0
  | sumall (Node (x, y, z)) =
      sumall x + y + sumall z;
```
```
(*
 * Code a function that converts the data in the tree into a list:
 *)
fun listall Empty = nil
  | listall (Node (x, y, z)) = listall x @ y :: listall z;
```
```
(*
 * Code a function that searches a tree for some particular element:
 *)
fun isintree x Empty = false
  | isintree x (Node(left,y,right)) =
      x = y orelse isintree x left orelse isintree x right;
```
```
(*
 * Code a datatype that describes arithmetic expressions with multiplication
 * and addition.
 *)
datatype exp = ADD of exp * exp | MUL of exp * exp | NUM of int
```
```
(*
 * Code a function that interprets these arithmetic expressions.
 *)
fun interp (NUM n) = n
  | interp (ADD (e1, e2)) = interp e1 + interp e2
  | interp (MUL (e1, e2)) = interp e1 * interp e2;
```
Example:
```
- val x = ADD ( (NUM 3), (NUM 4) );
- val y = MUL (x, (NUM 5));
- interp y;
```

So, in the end, functional programming is very easy. In general we have:
- A function f gets a type T, and returns a type R
- For every pattern that constitutes T, we must provide an implementation to f.
