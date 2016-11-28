# Semantics of Programming Languages

-----
What does the program below do?
```java
public class T {
  public static void main(String args[]) {
    int x = 1;
    x += (x = 2);
    System.out.println("x = " + x);
  }
}
```
> Print `x = 4`

What about this program?
```c
#include <stdio.h>
int main() {
  int x = 1;
  x += (x = 2);
  printf("x = %d\n", x);
}
```
> Print `x = 3`

-----
How to describe a programming language?

-----
Does anybody remember the grammar for simple integer expressions?
```
<exp> ::= <exp> + <mulexp> | <mulexp>
<mulexp> ::= <mulexp> * <rootexp> | <rootexp>
<rootexp> ::= ( <exp> ) | <constant>
```

-----
What is the associativity/precendence of the operators?

-----
What is the parse tree for `1 + 2 * 3`? What about **AST**?

-----
How to define the meaning of this language?

* The interpreter for the AST:
```
<ast> ::= plus(<ast>, <ast>)
        | times(<ast>, <ast>)
        | const (<const>)

val1(plus(X,Y),Value) :-
  val1(X,XValue),
  val1(Y,YValue),
  Value is XValue + YValue.
val1(times(X,Y),Value) :-
  val1(X,XValue),
  val1(Y,YValue),
  Value is XValue * YValue.
val1(const(X),X).
```

-----
What is `val1(const(1000000000), X)`?

-----
What is the problem of defining a language's semantics by an interpreter?

* Defining the semantics of this language means defining a map from AST's to values, e.g: `times(const(2), const(3)) -> 6`

* Natural semantics will define the relation of AST's to values. E.g:
```
 E1 -> v1      E2 -> v2         E1 -> v1      E2 -> v2
------------------------      --------------------------
plus(E1, E2) -> v1 + v2        times(E1, E2) -> v1 * v2

const(n) -> eval(n)
```

What is the meaning of this notation? The arrow and the fraction line?

* Natural semantics is also called big-step semantics. There is also the small-step semantics:

```
          E1 -> E1'                             E1 -> E1'
-----------------------------      ---------------------------------
plus(E1, E2) -> plus(E1', E2)        times(E1, E2) -> times(E1', E2)

          E2 -> E2'                             E2 -> E2'
-----------------------------      ---------------------------------
plus(E1, E2) -> plus(E1, E2')        times(E1, E2) -> times(E1, E2')

       E -> E'
--------------------
const(E) -> eval(E')

plus(v1, v2) -> v1 + v2      times(v1, v2) -> v1 * v2      const(v) -> eval(v)
```

-----
Would it be possible to change these rules so that addition would be left associative?

```
          E1 -> E1'                             E2 -> E2'
-----------------------------       -----------------------------
plus(E1, E2) -> plus(E1', E2)        plus(v, E2) -> plus(v, E2')
```

-----
How to define the semantics of `if/then/else` in **SML**?

- There may be more than one evaluation for a rule:

```
E1 -> true       E2 -> v2            E1 -> false      E3 -> v3
-------------------------     or     -------------------------
  if(E1, E2, E3) -> v2                 if(E1, E2, E3) -> v3
```

* Let's add variables to our toy language:
```
<exp>     ::= <exp> + <mulexp> | <mulexp>
<mulexp>  ::= <mulexp> * <rootexp> | <rootexp>
<rootexp> ::= let val <variable> = <exp> in <exp> end
           | ( <exp> ) | <variable> | <constant>
```

-------
How does the new constructions change the grammar for **AST**'s?
```
<ast> ::= plus (<ast>, <ast>)
        | times (<ast>, <ast>)
        | const (<const>)
        | let (<name>, <ast>, <ast>)
        | var (<name>)
```

-------
How to augment the interpreter so that it handles variables?

```
val2(plus(X, Y), Context, Value) :-
  val2(X, Context, XValue),
  val2(Y, Context, YValue),
  Value is XValue + YValue.
val2(times(X, Y), Context, Value) :-
  val2(X, Context, XValue),
  val2(Y, Context, YValue),
  Value is XValue * YValue.
val2(const(X), _, X).
val2(var(X), Context, Value) :-
  lookup(X, Context, Value).
val2(let(X, Exp1, Exp2), Context, Value2) :-
  val2(Exp1, Context, Value1),
  val2(Exp2, [bind(X, Value1) | Context], Value2).
```

How to give the PL the block semantics?

```
lookup(Variable, [bind(Variable, Value)|_], Value).
lookup(VarX, [bind(VarY, _)|Rest], Value) :-
  VarX \= VarY, lookup(VarX, Rest, Value).
```

* Example 1: `val2(let(y, const(3), times(var(y), var(y))), nil, X).`
* Example 2:
```
let val y = 3 in            val2(let(y,const(3),
  let val x = y * y in        let(x, times(var(y), var(y)),
    x * x                       times(var(x), var(x)))),
  end                       nil, X).
end
```

-------
Can you give me some other programs?

What is the meaning of
```
let val y = 1 in
  let val y = 2 in
    y
  end
end?
```

And what is the meaning of?
```
let val x = 1 in let val x = 2 in x end + x end
```
e.g.: `val2(let(x, const(1), plus(let(x, const(2), var(x)), var(x))), nil, V).`

-------
How to rewrite this new interpreter using a natural semantics notation?
```
(E1, C) -> v1    (E2, C) -> v2             (E1, C) -> v1     (E2, C) -> v2
------------------------------             -------------------------------
 (plus(E1, E2), C) -> v1 + v2               (times(E1, E2), C) -> v1 * v2


(var(v), C) -> lookup(C, v)                (const(n), C) -> eval(n)


(E1, C) -> v1    (E2, [bind(x, v1)|C]) -> v2
--------------------------------------------
        (let(x, E1, E2), C) -> v2
```


-------
What is the meaning of: `let val a = 1 in b end` ? E.g:  
`val2(let(a,const(1),var(b)),nil,X).`

What is wrong with the program above?  
How a compiler would handle such codes?

* Static semantics: program meaning known at compilation time.
* Dynamic semantics: program meaning known at run time.

-------
Which examples of errors of static semantics can you give me?
- references must be in the scope of definitions.
- variables must be used according to their declared type.

-------
Which examples of errors of dynamic semantics can you give me?

* Semantic equivalence
-------
What does it mean to say that two programs `P1` and `P2` are equivalent?
```
<P1, B> -> <v, B'> iff <P2, B> -> <v, B'>
```
- or -
```
<P1, B> -> stuck iff <P2, B> -> stuck
```

-------
Prove that
```
if be then c1 else c2 == if not(be) then c2 else c1
```

The proof is by induction on the rules used to evaluate the expression.

Case 1)
```
be -> true       c1 -> v
------------------------
  if(be, c1, c2) -> v
```

What we know? We know that (i) `be -> true` and (ii) `c1 -> v`  
From (i) and rule **NotTrue** we know that `not(be) -> false`, thus:

```
not(be) -> false       (ii) c1 -> v
-----------------------------------
    if(not(be), c2, c1) -> v
```

Case 2)
```
be -> false      c2 -> v
------------------------
  if(be, c1, c2) -> v
```

What we know? We know that (i) `be -> false` and (ii) `c2 -> v`
From (i) plus **NotFalse** we know that `not(be) -> true`, thus:

```
not(be) -> true     (ii) c2 -> v
--------------------------------
   if(not(be), c2, c1) -> v
```


-------
How to add functions to our grammar?
```
<exp>     ::= fn <variable> => <exp> | <addexp>
<addexp>  ::= <addexp> + <mulexp> | <mulexp>
<mulexp>  ::= <mulexp> * <funexp> | <funexp>
<funexp>  ::= <funexp> <rootexp> | <rootexp>
<rootexp> ::= let val <variable> = <exp> in <exp> end
           | ( <exp> ) | <variable> | <constant>
```

-------
Can anyone give me examples of programs in this language?
```
(fn x => x * x) 3
```

What is the parse tree of this program above?

-------
How many parameters can a function have?

-------
How to model this new AST?
```
- apply(Function, Actual)
- fn(Formal, Body)

val3(plus(X, Y), Context, Value) :-
  val3(X, Context, XValue),
  val3(Y, Context, YValue),
  Value is XValue + YValue.

val3(times(X, Y), Context, Value) :-
  val3(X, Context, XValue),
  val3(Y, Context, YValue),
  Value is XValue * YValue.

val3(const(X), _, X).

val3(var(X), Context, Value) :-
  lookup(X, Context, Value).

val3(let(X, Exp1, Exp2), Context, Value2) :-
  val3(Exp1, Context, Value1),
  val3(Exp2, [bind(X, Value1) | Context], Value2).

val3(fn(Formal, Body), _, fval(Formal, Body)).

val3(apply(Function, Actual), Context, Value) :-
  val3(Function, Context, fval(Formal, Body)),
  val3(Actual, Context, ParamValue),
  val3(Body, [bind(Formal, ParamValue)|Context], Value).

lookup(Variable, [bind(Variable, Value)|_], Value).
lookup(VarX, [bind(VarY, _)|Rest], Value) :-
  VarX \= VarY, lookup(VarX, Rest, Value).
```

-------
What is the value of:
```
let val x = 1 in
  let val f = fn n => n + x in
    let val x = 2 in
      f 0
    end
  end
end
```

E.g.:
```
      val3(let(x, const(1), let(f, fn(n, plus(var(n), var(x))), let(x,
      const(2), apply(var(f), const(0))))), nil, X).
```

Represent the context graphically.

Is this static or dynamic scoping?

-------
How to add static scoping?
- Add to the description of functions a copy of the context where that function is defined.

```
val4(plus(X, Y), Context, Value) :-
  val4(X, Context, XValue),
  val4(Y, Context, YValue),
  Value is XValue + YValue.

val4(times(X, Y), Context, Value) :-
  val4(X, Context, XValue),
  val4(Y, Context, YValue),
  Value is XValue * YValue.

val4(const(X), _, X).

val4(var(X), Context, Value) :-
  lookup(X, Context, Value).

val4(let(X, Exp1, Exp2), Context, Value2) :-
  val4(Exp1, Context, Value1),
  val4(Exp2, [bind(X, Value1) | Context], Value2).

val4(fn(Formal, Body), Context, fval(Formal, Body, Context)).

val4(apply(Function, Actual), Context, Value) :-
  val4(Function, Context, fval(Formal, Body, Nesting)),
  val4(Actual, Context, ParamValue),
  val4(Body, [bind(Formal, ParamValue)|Nesting], Value).

lookup(Variable, [bind(Variable, Value)|_], Value).
lookup(VarX, [bind(VarY, _)|Rest], Value) :-
  VarX \= VarY, lookup(VarX, Rest, Value).
```

What is a closure in this interpreter?

* Example:
```
let
  val f = fn x => let val g = fn y => y+x in g end
in
  f 1 2
end
```

E.g.:
```
        val4(let(f,fn(x,let(g,fn(y,plus(var(y),var(x))), var(g))),
        apply(apply(var(f),const(1)),const(2))),nil, X).
```

How do we simulate the fact that the activation record of this function cannot be deallocated after the function returns?

-------
How to write the semantic rules using the natural notation? Use dynamic scoping:
```
(E1, C) -> v1    (E2, C) -> v2             (E1, C) -> v1     (E2, C) -> v2
------------------------------             -------------------------------
 (plus(E1, E2), C) -> v1 + v2               (times(E1, E2), C) -> v1 * v2


(var(v), C) -> lookup(C, v)   (const(n), C) -> eval(n)   (fn(x, E), C) -> (x, E)


(E1, C) -> v1    (E2, [bind(x, v1)|C]) -> v2
--------------------------------------------
        (let(x, E1, E2), C) -> v2


(E1, C) -> (x, E3)     (E2, C) -> v1     (E3, [bind(x,v1)|C]) -> v2
-------------------------------------------------------------------
                     (apply(E1, E2), C) -> v2
```

-------
How is recursion implemented, e.g: `let val f = fn x => f x in f 1 end` ?  
What is the meaning of the recursive program below?  
```
val4(let( f, fn(x, apply(var(f), var(x))), apply(var(f), const(1))), nil, X).
```

Why is it tricky to have recursion under static scoping?

-------
What is the role of the evaluation order in this interpreter?

What is the Java semantics of: `x = 1; x += (x = 2)`?
```
x = 2;                oldx = x;               oldx = x;
oldx = x;             x = 2;                  x = oldx + 2;
x = oldx + 2          x = oldx + 2;           x = 2;
```
