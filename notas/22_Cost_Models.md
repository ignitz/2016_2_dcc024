# Cost Models

------
Let's remember how was the implementation of append in Prolog?
```
@([], L, L).
@([H|T], L, [H|Tt]) :- @(T, L, Tt).
```

Which one is faster? Assuming `l` is a list with 10.000.000 elements?

given `val l = [2,3,4]`:
```
- val a = (1::l);
- val a = l @ [1];
```

Which one is faster?
```
?- L = [1,2,3], A = [0|L].
L = [1, 2, 3],
A = [0, 1, 2, 3].

?- L = [1,2,3], append(L, [0], A).
L = [1, 2, 3],
A = [1, 2, 3, 0].
```

------
How do you think lists are implemented in SML or Prolog?

------
Represent it graphically:
```
?- D = [2, 3].
?- E = [1|D].
?- E = [F|G].
?- D = [2, 3].
```

------
How do we know that SML and Prolog share list structures?

Python has an append operator '+':
```
>>> l = [1,2,3]
>>> a = l + [4]
>>> l
[1, 2, 3]
>>> a
[1, 2, 3, 4]
```

How do we know if '+' reuses the elements of both or some of the lists, or copies them again?

------
How was the implementation of length in SML?
```
fun length nil = 0
    length (head::tail) = 1 + length tail;
```

What's its complexity?

------
How was the implementation of append in prolog?
```
append([],X,X).
append([Head|Tail], X, [Head|Suffix])
 :-  append(Tail, X, Suffix).
```

What's its complexity?

Represent it graphically:
```
H = [1,2],
I = [3,4],
append(H, I, J).
```

------
What is the complexity of unifying lists?
```
?- K = [1,2],
|    M = K,
|    M = [1,2].
```

Represent this program graphically.

Implement the predicate `xequal(X, Y)`, that is true when `X` and `Y` represent the same list:
```
&=([], []).
&=([H|Tl], [H|Tr]) :- &=(Tl, Tr).
```

Which one is faster: to unify two lists, or to compute the length of two lists?

Any optimization?

* Cons-Cell Sumarry:
  - Consing takes constant time.
  - Extracting head or tail takes constant time.
  - Computing the length of a list takes time proportional to the length.
  - Computing the result of appending two lists takes time proportional to the length of the first list.
  - Comparing two lists, in the worst case, takes time proportional to their size.

------
What's the complexity of reverse?

```
reverse([],[]).
reverse([Head|Tail],Rev)
 :-  reverse(Tail,TailRev),  append(TailRev,[Head],Rev).
```

How to do it better?
```
reverse(X,Y) :- rev(X,[],Y).
rev([],Sofar,Sofar).
rev([Head|Tail],Sofar,Rev)
 :-  rev(Tail,[Head|Sofar],Rev).
```

How to create a list of 5000 elements in Prolog?
- Try it: `length(X, 5000), reverse(X, Y).`

What about in SML?
```
- fun rev [] acc = acc
=   | rev (h::t) acc = rev t (h::acc);
- fun reverse l = rev l [];
```

Draw the activation records of `reverse [1,2]`
```
rev -> head: 1
       tail: [2]
       sofar: nil
       ret addr
       P.A.R.
       Res: ?
```

Which optimizations are possible in this sequence of calls?

------
Which implementation is more efficient:
```
- fun sum [] = 0
=   | sum (a::l) = sum l + a;
- or -
- fun sum [] acc = acc
=   | sum (a::l) acc = sum l (a + acc);
```

* When a function makes a tail call, it no longer needs its activation record

-------
Is it tail call?
```
fun sum [] = 0
  | sum (h::t) = h + sum t
```

How to convert to a tail call function?

-------
How to convert length below to tail call?
```
fun length nil = 0
|   length (head::tail) =
       1 + length tail;

fun length thelist =
    let
      fun len (nil,sofar) = sofar
      |   len (head::tail,sofar) =
             len (tail,sofar+1);
    in
      len (thelist,0)
    end;
```

-------
Is `r` a tail call in the predicate below?

```
p :- q(X), r(X).
```

- Not necessarily, due to backtracking.

-------
Why is the predicate below inefficient?
```
grandfather(X,Y) :- parent(X,Z), parent(Z,Y), male(X).
```

e.g:
```
parent(d, a).      parent(d,a) -> parent(a,b) -> male(d) -> V
parent(f, e).      parent(f,e) -> parent(e,b) -> male(f) -> X
parent(a, b).      parent(f,e) -> parent(e,b) -> male(f) -> X
parent(e, b).      parent(a,b) -> X
parent(e, c).      parent(e,b) -> X
male(d).           parent(e,c) -> X
male(a).
```

How to improve it?
```
grandfather(X,Y) :- male(X), parent(X,Z), parent(Z,Y).
```

e.g:
```
parent(d, a).      male(d) -> parent(d,a) -> parent(a, b) -> X
parent(f, e).      male(a) -> parent(a,b) -> X
parent(a, b).      
parent(e, b).      
parent(e, c).      
male(d).           
```

What is a general guideline to code efficient Prolog searches?

-------
How are arrays stored in memory?

-------
What is the result of the **matrix.c** program?

* There are two main allocation schemes for arrays:
  - row-major order: C, C++.
  - column-major order: Fortran

-------
Does a C program still work if the allocation scheme changes?

-------
How to index data in a `m*n` bi-dimensional array allocated in row major order?
```
A[i, j] = base + (i * n * size) + (j * size)
```

What if the `m*n` array is column major?
```
A[i, j] = base + (i * size) + (j * m * size)
```

-------
Which way to sum up the elements in the matrix is faster?
```c

#include "stdio.h"
int main() {
  const int M = 5000;
  const int N = 1000;
  char m[M][N];
  int i, j, k;
  int sum = 0;
  // Initializes the array:
  for (i = 0; i < M; i++) {
    for (j = 0; j < M; j++) {
      m[i][j] = (i + j) & 7;
    }
  }
  // Sums up the matrix elements, row major:
  for (i = 0; i < M; i++) {
    for (j = 0; j < N; j++) {
      sum += m[i][j];
    }
  }
  // Sums up the matrix of elements, column major:
  for (j = 0; j < N; j++) {
    for (i = 0; i < M; i++) {
      sum += m[i][j];
    }
  }
  printf("The sum is %d\n", sum);
}
```

-------
How are arrays organized in Java?

-------
How to speed up the program below?
```c
// user   0m0.853s
#include "stdio.h"

int max(int i, int j) {
  return i > j ? i : j;
}

int main() {
  int i, j;
  double sum = 0.0;
  for (i = 0; i < 10000; i++) {
    for (j = 0; j < 10000; j++) {
      sum += max(i, j);
    }
  }
  printf("%d\n", sum);
}
```

-------
Is inlining a case for macros over functions, after all?

-------
How to code efficient programs, after all?
