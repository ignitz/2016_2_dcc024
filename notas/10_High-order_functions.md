# High-order functions (Aula 10)

### Currying:

1) How to write a function that adds two numbers in SML?
a) ``fun addp (x, y) = x + y;``
b) ``fun add x y = x + y;``

2) What is the difference between ``addp`` and ``add``?

3) How is ``b`` in lambda-calculus?
$\lambda x . \lambda y . x + y$

4) What is ``add 1``?
$\lambda y . 1 + y$

5) How to write a function that ``adds 1`` to a number?
``fun x = x + 1``
or
``val inc2 = add 1;``

### Anonymous functions in SML:

6) What is this declaration doing?
```
val inc = fn n => n + 1;
- inc 3
```

7) And this declaration, what is it doing?
```
val t = (fn n => n + 1, fn n => n - 1);
- #1 t 3
```
In SML, we can create anonymous functions, e.g: ``(fn x => x + 2) 3``

8) What is the difference between:
```
fun f x = x + 2
val f = fn x => x + 2
```

``fun`` is just a syntactic sugar (were it not for the recursive functions)!
``fn`` is the equivalent of lambdas in SML, e.g:
$\lambda x.x$ is equivalent to ``fn x => x``

8.1) Unless we need recursive functions: there is no way to define anonymous recursive functions. Why?

9) How we had defined numbers in Lambda-calculus?

```
val ZERO = fn s => fn z => z;
val ONE = fn s => fn z => s z;
val TWO = fn s => fn z => s (s z);
val THREE = fn s => fn z => s (s (s z));
...
```

10) Code a function that converts a number from the functional format into an integer:
```
fun inc n = n + 1;
fun P x = x inc 0;
- P ONE;
```

10.1) How to create a function that converts a number in the functional format into a string?
```
fun IS x = Int.toString (P x);
```

11) Does anybody remember how the successor function was implemented?
```
val SUCC = fn w => fn y => fn x => y(w y x);
- P (SUCC TWO);
```

12) Does anybody remember how to implement additions?
```
val ADD = fn n1 => fn n2 => n1 SUCC n2;
- P (ADD ONE TWO);
```

13) How to implement multiplication?
```
val MUL = fn n1 => fn n2 => fn z => n1 (n2 z);
- P (MUL TWO THREE);
```

14) Does anybody remember how to code boolean values?
```
val T = fn x => fn y => x;
val F = fn x => fn y => y;
```

15) How to code a function that converts booleans in the functional format to bool?
```
fun PB x = x true false;
```

16.1) How to code a function that converts booleans in the functional format to strings?
```
fun BS b = Bool.toString (PB b);
```

17) What about the boolean logical functions?
```
val AND = fn x => fn y => x y F;
- PB (AND T F);
- PB (AND T T);
val OR = fn x => fn y => x T y;
- PB (OR T F);
- PB (OR F F);
val NOT = fn x => x F T;
- PB (NOT T);
- PB (NOT F);
```

18) How to code a simple conditional test?
```
val IF = fn b => fn x => fn y => b x y;
- IF T 1 2;
- IF F 1 2;
```

### Higher-order functions:

Each function has an order:
- A function that does not take other functions as parameters, and does not return a function value, has order 1
- A function that takes a function as a parameter or returns a  function value has order n+1, where n is the order of its  highest-order parameter or returned value

19) Is it possible to pass functions as parameters in C?

20) What is the order of functions with each of the following ML types?
```
int * int -> bool
int list * (int * int -> bool) -> int list
int -> int -> int
(int -> int) * (int -> int) -> (int -> int)
int -> bool -> real -> string
```

### Predefined Higher-Order functions: map, foldr, foldl

map takes a function ``f`` and a ``list l``, and applies ``f`` on every element of ``l``:

21) What does the functions below do?
```
map (fn x => x + 1) [1, 2, 3, 4];
map (fn x => x mod 2 = 0) [1, 2, 3, 4];
map ~ [1,2,3,4];
map (op +) [(1,2),(3,4),(5,6)];
```

21.1) Code a function that converts a string to a list of integers
representing the ASCII values of the characters:
```
fun asc s = map ord (explode s);
```

22) What is the type of the map function?

22.1) And what is the type of this function? ``val map_sum = map (op +)``?

foldr takes a binary function ``op``, an initial value ``c``, and a ``list``

``[x1, x2, ..., xn]``, and returns:
``x1 op (x2 op (... (xn op c)))``, for instance:
``foldr (op +) 0 [1, 2, 3, 4]``.

23) What do each of these function calls do?
```
foldr (op +) 0 [1,2,3,4];
foldr (op * ) 1 [1,2,3,4];
foldr (op ^) "" ["abc","def","ghi"];
foldr (op ::) [5] [1,2,3,4];
```

24) How to write an append function using ``foldr``?
```
fun append l1 l2 = foldr (op ::) l2 l1;
```

Example:
```
- foldr (op ::) [3,4] [1,2];
val it = [1,2,3,4] : int list
```

25) What is the type of the following functions?
```
foldr;
val it = fn : ('a * 'b -> 'b) -> 'b -> 'a list -> 'b

foldr (op +);
foldr (op +) 0;
val sumList = foldr (op +) 0;
```

``foldl`` takes a binary function ``op``, an initial value ``c``, and a ``list``

``[x1, x2, ..., xn]``, and returns:
``(xn op ... (x2 op (x1 op c))...)``
For instance, ``foldl (op +) 0 [1,2,3,4]`` evaluates as ``4+(3+(2+(1+0)))=10``,
but ``foldr did 1+(2+(3+(4+0)))=10``.

26) Is it true of false?
```
foldr (op ^) "" ["abc","def","ghi"] = foldl (op ^) "" ["abc","def","ghi"];
foldr (op +) 0 [1,2,3,4] = foldl (op +) 0 [1,2,3,4];

foldr (op -) 0 [1,2,3,4] = foldl (op -) 0 [1,2,3,4];
```

E.g: ``1 - (2 - (3 - (4 - 0))) != 4 - (3 - (2 - (1 - 0)))``

27) When do ``foldr`` and ``foldl`` return the same value?

27.1) Can you think about operations that are associative, but not commutative?
What about a full table?
Assoc | Commut | Function
|---|---|---|
    1   |    1  | ``fn(x, y) => x + y``
    1   |    0  | ``fn(x, y) => x ^ y`` (* String concat \*)
    0   |    1  | ``fn(x, y) => not(x andalso y)``
    0   |    0  | ``fn(x, y) => x - y``

27.2) For which operations we have ``foldr == foldl``?
```
foldr cct "" ["a", "b", "c"]  <> foldl cct "" ["a", "b", "c"]
foldr sub 0 [1,2,3,4]         <> foldr sub 0 [1,2,3,4]
foldr nand true [true, false] <> foldl nand true [true, false]
foldr sum 0 [1,2,3,4]          =  foldl sum 0 [1,2,3,4]
```

Applications:
```
(*
 * This program solves problem 10 from the Euler project, using the Sieve of
 * Erastothenes.
 *
 * val ans = foldr (op +) (sieve 2000000)
 *)
fun filter _ nil = nil
  | filter f (h::t) = if f h then h :: filter f t else filter f t

fun range 1 = nil
  | range (n:IntInf.int) = n :: range (n-1)

fun inv nil acc = acc
  | inv (h::t) acc = inv t (h::acc)

fun filterNonPrimes _ nil = []
  | filterNonPrimes limit (h::t) =
      if (IntInf.toLarge h) * h <= limit
      then h :: filterNonPrimes limit (filter (fn e => (e mod h) <> 0) t)
      else (h::t)

fun sieve n =  filterNonPrimes n (inv (range n) nil)

(*
 * An elegant implementation of quicksort.
 *)
fun leq a b = a <= b

fun grt a b = a > b

fun filter _ nil = nil
  | filter f (h::t) = if f h then h :: filter f t else filter f t

fun qsort nil = nil
  | qsort (h::t) = (qsort (filter (grt h) t)) @ [h] @ (qsort (filter (leq h) t))
```
