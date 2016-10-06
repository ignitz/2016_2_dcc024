Introduction to ML (Aula 05)
==================

The last class:  
Interpreter: swipl  
Compiler: gcc  
Assembler: as  
Linker: ld  
Loader: operating system.  
Virtual Machine: JVM  
JIT compiler: Firefox's traceMonkey.

ML was designed by Robin Milner and others on the early seventies.

ML has many dialects, we will be using SML, which stands for Standard ML.

We will be using mostly interpreted ML.  
The interpreter gives us an interactive programming environment.
```
- 4 + 3;
val it = 7: int
```

1) What we notice here?
- type inference.
- variable it.
- The ending semi-collon

```
- 1.4;
val it = 1.4 : real
```

2) In ML, the unary minus is ~, like ~1.4. Why not to use the simple hyphen?

Examples of int, real and boolean expression:
```
- true;
val it = true : bool
- 1.4;
val it = 1.4 : real
- 14;
val it = 14 : int
```

3) What are primitive types?
In ML strings are primitive:
```
- "dcc024";
```

To make characters, we use #:
```
- #"a";
```

There are many arithmetic operators in ML:
```
- ~ 1 + 2 - 3 * 4 div 5 mod 6;
val it = ~1: int
- ~ 1.0 + 2.0 - 3.0 * 4.0 / 5.0;
```

4) What is the relative precedence between `+ , - , * , / , div , mod , ~ `?

Other operators:

```
- "bibity" ^ "bobity" ^ "boo";
val it = "bibitybobityboo" : string
- 2 < 3;
val it = true : bool
- 1.0 <= 1.0;
val it = true : bool
- #"d" > #"c";
val it = true : bool
- "abce" >= "abd";
val it = false : bool
```

5) What is the value of:
```
- 1.3 = 1.3;
```

5.1) What about
```
- 1.3 <> 1.3;
```

Some more operands:
```
- 1 < 2 orelse 3 > 4;
val it = true : bool
- 1 < 2 andalso not (3 < 4);
val it = false : bool
```

6) How to compare two real numbers?
```
- fun rcomp a (b:real) = not (a < b orelse a > b);
val rcomp = fn : real -> real -> bool
- rcomp 3.0 3.0;
val it = true : bool
```

7) How is our precedence table so far?
```
{orelse} < {andalso} < {=,<>,<,>,<=,>=} < {+,-,^} < {*,/,div,mod} < {~,not}
```

8) What is the value of `1 div 0;`?  
8.1) What is the value of: `- true orelse 1 div 0 = 0;`?

SML has conditional expressions:
```
- if 1 < 2 then #"x" else #"y";
val it = #"x" : char
- if 1 > 2 then 34 else 56;
val it = 56 : int
- (if 1 < 2 then 34 else 56) + 1;
val it = 35 : int
```

9) In SML, every if has a then and an else. Why the designers of the
language prefer not to have a if without an else?

10) What is the difference between an expression and a command?

11) What is the type and the value of each of these expressions?

```
1 * 2 + 3 * 4;
"abc" ^ "def";
if (1 < 2) then 3.0 else 4.0;
1 < 2 orelse (1 div 0) = 0;
```

12) What is wrong with each of these expressions?
```
10 / 5;
#"a" = #"b" or 1 = 2;
1.0 = 1.0;
if (1<2) then 3
```

SML has overloading:
```
- 1 * 2;
val it = 2 : int
- 1.0 * 2.0;
val it = 2.0 : real
```

11) What is the value of:
```
- 1.0 * 2;
```

There are built-in conversion functions:
```
- real(123);
val it = 123.0 : real
- floor(3.6);
val it = 3 : int
- floor 3.6;
val it = 3 : int
- str #"a";
val it = "a" : string
```

12) What is the value of:
```
- fun square x = x * x;
- square 2+1;
```

12.1) What is the associativity of function application?

13) What is wrong with the functions below?
```
trunc 5
ord "a"
if 0 then 1 else 2
if true then 1 else 2.0
chr trunc 97.0, but notice "chr(trunc(97.0)); chr(trunc 97.0)"
```

We can bind values to names in SML:
```
- val x = 1+2*3;
val x = 7 : int
- x;
val it = 7 : int
```

14) What is the difference between variables in SML and C?
- See, for instance, this program:
```c
int main() {
  int x = 1;
  int* p = &x;
  *p = 2;
  printf("%d\n", x);
}
```
<!-- ** -->


15) What is the value and type of each of these declarations?
```
val a = "123";
val b = "456";
val c = a ^ b ^ "789";
val a = 3 + 4;
- a;
- b;
- c;
```

16) What is '**it**' here?
```
- 2 + 3;
val it = 5
```

ML has tuples:
```
- val barney = (1+2, 3.0*4.0, "brown");
val barney = (3,12.0,"brown") : int * real * string
```

17) What is the ' * '?

```
- val point1 = ("red", (300,200));
val point1 = ("red",(300,200)) : string * (int * int)
- #2 barney;
val it = 12.0 : real
```

17.1) How to get the first element of the tuple inside point1?
```
- #1 (#2 point1);
val it = 300 : int
```

17.2) What is the value of:
```
- #1 (1, 2);
val it = 1 : int
```

17.3) What is the value of:
```
- #1 (1);
```


18) What is the difference between:
```
int * (int * bool) and (int * int) * bool?
```

One of the most important data structures is the list, e.g:
```
- [1,2,3];
val it = [1,2,3] : int list
- [1.0,2.0];
val it = [1.0,2.0] : real list
```

19) What is the type of the declarations below?
```
- [true];
val it = [true] : bool list
- [(1,2),(1,3)];
val it = [(1,2),(1,3)] : (int * int) list
- [[1,2,3],[1,2]];
val it = [[1,2,3],[1,2]] : int list list
```

20) What is the difference between lists and tuples?

There is an empty list:
```
- [];
val it = [] : 'a list
- nil;
val it = [] : 'a list
```

21) What is the type of nil?

22) What is the type constructor for lists?

There is a function to test if a list is null:
```
- null [];
val it = true : bool
- null [1,2,3];
val it = false : bool
```

There is a function to concatenate lists:
```
- [1,2,3]@[4,5,6];
val it = [1,2,3,4,5,6] : int list
```

23) What is the complexity of '@'?

There is a constructor for lists:
```
- val x = #"c"::[];
val x = [#"c"] : char list
- val y = #"b"::x;
val y = [#"b",#"c"] : char list
- val z = #"a"::y;
val z = [#"a",#"b",#"c"] : char list
```

24) What is the complexity of ::?

25) Consider: `- val z = 1::2::3::[]`;
What is the associativity of ::?

There are special functions to manipulate lists of characters:
```
- explode "hello";
val it = [#"h",#"e",#"l",#"l",#"o"] : char list
- implode [#"h",#"i"];
val it = "hi" : string
```

There are two important functions to manipulate lists:
**hd** and **tl**:
```
- hd;
val it = fn : 'a list -> 'a
- tl;
val it = fn : 'a list -> 'a list
```

26) What is the type of **hd**?

26.1) What is the type of **tl**?

27) What are the values of these expressions?
```
#2(3,4,5)
hd(1::2::nil)
hd(tl(#2([1,2],[3,4])));
```

28) What is wrong with the following expressions?
```
1@2
hd(tl(tl [1,2]))
[1]::[2,3]
```

We build programs in ML using functions:

29) How to get the first character of a list?
```
- fun firstChar s = hd (explode s);
val firstChar = fn : string -> char
```

30) How does ML finds the type of the function?  
30.1) What is the type constructor of functions?
```
- firstChar "abc";
val it = #"a" : char
```

Every function in SML takes exactly one parameter:
```
- fun quot(a,b) = a div b;
val quot = fn : int * int -> int
- quot (6,2);
val it = 3 : int
- val pair = (6,2);
val pair = (6,2) : int * int
- quot pair;
val it = 3 : int
```

31) Can you define a factorial function?
```
fun fact n = if n < 2 then 1 else n * fact(n-1);
```

32) Can you define a function to sum all the elements of a list?
```
fun listsum x = if null x then 0 else hd x + listsum(tl x);
```

32.1) What is the type of this function?

33) Define a function to compute the length of a list:
```
fun length x = if null x then 0 else 1 + length (tl x);
```

Consider this function:
```
fun badlength x = if x=[] then 0 else 1 + badlength (tl x);
```

34) What is the value of `badlength [1.0, 2.0]`?

How to fix badlength? With null x.

35) Produce a function to reverse a list:
```
fun reverse L = if null L then nil else reverse(tl L) @ [hd L];
```

36) Which primitive types have we seen so far?
```
int, real, bool, char and string.
```

37) Which type constructors? `* , list and ->`

38) What is the precedence in:
```
int * bool list -> real
Ans: same as (int * (bool list)) -> real
```

39) What is the type of `fun prod(a,b) = a * b;`?  
39.1) Why int instead of real?  
39.2) How to force real?
```
fun prod(a,b):real = a * b;
fun prod(a:real,b) = a * b;
fun prod(a,b:real) = a * b;
fun prod(a,b) = (a:real) * b;
fun prod(a,b) = a * b:real;
fun prod(a,b) = (a*b):real;
fun prod((a,b):real * real) = a*b;
```

Escreva uma funcao max, que retorne o maior elemento de uma lista.
Exemplo:
```
- max [2, 3, 1, 0];
val it = 3
```

```
fun max l =
  if null l
  then 0
  else
    if hd l > max (tl l)
    then hd l
    else max (tl l)
```

```
fun max2 nil = 0
  | max2 (a::al) =
      let
        val vmax = max2 al
      in
        if a > vmax then a else vmax
      end
```

Escreva uma funcao zip, que receba duas listas l1 e l2, e retorne uma lista de
pares, onde o primeiro elemento provem de l1, e o segundo de l2. Por exemplo:
```
- zip [1, 2, 3] ["a", "b", "c"];
val it =  [(1, "a"), (2, "b"), (3, "c")]
```

```
fun zip l1 l2 =
  if null l1
  then nil
  else
    if null l2
    then nil
    else (hd l1, hd l2) :: zip (tl l1) (tl l2)
```

```
fun zip2 nil l = nil
  | zip2 l nil = nil
  | zip2 (a::al) (b::bl) = (a,b) :: zip2 al bl
```

Escreva uma funcao `fib n`, que retorne o n-esimo numero da sequencia de
Fibonacci.
```
fun fib n =
  if n = 0
  then 0
  else
    if n = 1
    then 1
    else fib (n - 1) + fib (n - 2)
```

```
fun fib 0 = 0
  | fib 1 = 1
  | fib n = (fib (n-1)) + (fib (n-2)) ;

fun fibsFrom 0 = [0]
  | fibsFrom 1 = [1, 0]
  | fibsFrom n =
    let
      val a::b::l = fibsFrom (n-1)
    in
      ( a + b ) :: (a::b::l)
    end ;

fun fastFib n = hd (fibsFrom n) ;
```
