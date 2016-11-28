# Introduction to Prolog

* Prolog is a logic programming language. A program is a set of facts and logic rules, and computation happens by deriving other facts from these rules.
* Origin: French, 1971/72, by Alain Colmerauer and Phillipe Roussel.
* Motivation: natural language processing.

* Basis: Horn Clauses:
  (p and q and ... and t) -> u  
  is written, in Prolog, as:  
  `u :- p, q, ..., t.`

* Prolog has very simple syntax.
* The core of prolog is the term:
 - atoms:
  - constants: 123, -123, etc
  - alpha-numeric symbols: fernando, * , . , = , []
  - atoms start with lower case.
 - variable: names beginning with an upper-case letter:  
 `X, Fernando, _`
 - composite terms: an atom plus a sequence of terms between parentheses:  
 `x(Y, Z), x(y, Z)`

- A prolog program knows facts.
  - A fact is a term followed by a period: parent(kim, holly).
  - A fact is like a declaration, an axiom.
  - Example:

```
  parent(kim, holly).
  parent(margaret, kim).
  parent(margaret, kent).
  parent(esther, margaret).
  parent(herbert, margaret).
  parent(herbert, jean).
```

* I can fill a file with facts, and load it in the prolog interpreter using the clause consult.
```
?- consult(par).
- or -
$> swipl -g "consult(par)."
```

------
Do you remember the difference between interpreters and compilers?

* After consulting, the interpreter 'knows' the facts as true.
* The user interacts with the interpreter by means of queries.

------
What is the meaning of typing the following in the interpreter:
```
?- parent(kim, holly).
?- parent(kim, X).
?- parent(X, Y).
?- parent(Y, esther).
```

* a query can be a sequence of terms:

------
What is the result of the query:
```
?- parent(margaret, X), parent(X, holly).
```
What is the scope of `X`?

------
How to discover if `esther` has a `grand-grandchild`?
```
?- parent(esther, C), parent(C, GC), parent(GC, GGC).
```

* Programming languages normally provide abstractions to bind computation together. For instance, Java gives the method, SML gives the function, and C gives the procedure. Yet, these abstractions are pretty much the same thing.

Prolog gives you something more different: inference rules.

* A rule is a specification of how to derive new facts.
```
greatgrandparent(GGP, GGC) :- parent(GGP, GP), parent(GP, P), parent(P, GGC).
```

------
What does this rule say?

Can you define a rule `grandparent`?
```
grandparent(GP, GC) :- parent(GP, P), parent(P, GC).
```

Can you derive `greatgrandparent` in terms of `grandparent`?
```
greatgrandparent(GGP, GGC) :- grandparent(GGP, P), parent(P, GGC).
```

------
How to define the rule ancestor?
* Rules can be recursive:
```
ancestor(X, Y) :- parent(X, Y).
ancestor(X, Y) :- parent(Z, Y), ancestor(X, Z).
```

------
How to read:
```
greatgrandparent(GGP, GGC) :- parent(GGP, GP), parent(GP, P), parent(P, GGC).
```

- recipe (order matters)
- declaration (order does not matter)

* What is the result of this query:
```
?- X.
```

* Lists
```
?- X = [1, fernando, true].
?- [1,2|X] = [1,2,3,4,5].
```

------
How to create a predicate to append two lists?
```
append([], L, L).
append([Head|TailA], B, [Head|TailC]) :- append(TailA, B, TailC).
```

Ex.:
```
?- append([1,2,3], [4,5,6], X).
```

------
What is:
```
?- append(X, Y, [1,2,3]).
```

--------
How to build a predicate member?
```
member(X, [X|_]).
member(X, [_|Y])
  :- member(X, Y).

```

What is the meaning of ' _ '?

Example:
```
?- member(2, [1,2,3]).
?- member(2, [X,2,3]).
?- member(2, [1,X,3]).
?- member(4, [1,2,3]).
?- member(X, [1,2,3]).
?- member(1, X).
```

------
How to write a predicate select, so that `select(E, L1, L2)` is true if, and only if, L2 is the same as L1, minus an element E?
```
select(X, [X|L], L).
select(X, [H|L], [H|LR])
  :- select(X, L, LR).
```

Examples:
```
?- select(2, [1,2,3], Z).
?- select(2, [1,3], Z).
?- select(X, [1,2,3], [2,3]).
?- select(X, [1,2,3], Y).
```

------
How to write a predicate reverse?
```
reverse([], []).
reverse([H|T], X)
  :- reverse(T, XAux), append(XAux, [H], X).
```

Examples:
```
?- reverse([1, 2, 3], L).
```

------
What happens in this case?
```
?- reverse(L, [1, 2, 3]).
```

* Negation as failure:
```
?- not(member(4, [1,2,3])).
```

------
How to write a relationship siblings(X, Y), that is true if, and only if, X and Y represent siblings in the parents database?
```
sibling(X,Y) :- parent(P,X), parent(P, Y).
?- sibling(kim, kent).
?- sibling(kent, Z).
... kent is not sibling of kent ...
```

------
How to fix the sibling predicate?
```
sibling(X,Y) :- not(X=Y), parent(P,X), parent(P, Y).
?- sibling(kim, kent).
?- sibling(kent, Z).
```

How to fix it?
```
sibling(X,Y) :- parent(P,X), parent(P, Y), not(X=Y).
```

------
How to code the predicate subset, such that `subset(S1, S2)` is true if S2 is a subset of S1?
```
subset([], []).
subset([H|T], [H|L]) :- subset(T, L).
subset([_|T], L) :- subset(T, L).
```

------
How to code the predicate intersec, such that intersec(L1, L2) is true if the intersection between L1 and L2 is non-empty.
```
intersec([H|_], L2) :- member(H, L2).
intersec([_|L1], L2) :- intersec(L1, L2).
```

------
How to code a predicate perm, such that `perm(T, L)` is true if L is a permutation of the list T?
```
perm([], []).
perm(T, L) :- member(H, T), select(H, T, L1), perm(L1, L2), L = [H|L2].
```

Can you make it shorter?
```
nperm([], []).
nperm([H|T], L) :- nperm(T, Lx), select(H, L, Lx).
```

------
Solve the problem of the man, cabbage, goat and wolf:
```
change(o, l).
change(l, o).

move([X, X, Goat, Cabbage], wolf, [Y, Y, Goat, Cabbage]) :- change(X,Y).
move([X, Wolf, X, Cabbage], goat, [Y, Wolf, Y, Cabbage]) :- change(X,Y).
move([X, Wolf, Goat, X], cabbage, [Y, Wolf, Goat, Y]) :- change(X,Y).

move([X, Wolf, Goat, Cabbage], nothing, [Y, Wolf, Goat, Cabbage]) :- change(X,Y).

oneEq(X, X, _).
oneEq(X, _, X).

safe([Man, Wolf, Goat, Cabbage]) :-
  oneEq(Man, Wolf, Goat),
  oneEq(Man, Goat, Cabbage).

solution([l, l, l, l], []).
solution(Config, [Move|Rest]) :-
  move(Config, Move, NextConfig),
  safe(NextConfig),
  solution(NextConfig, Rest).
```

------
What happen if I try the query below?
```
solution([o, o, o, o], X).
```

------
How to fix it?
```
length(X, 7), solution([o, o, o, o], X).
```
