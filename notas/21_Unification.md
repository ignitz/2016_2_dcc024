# Unification

* The heart of Prolog is an algorithm called 'Unification'.
 - Input: two terms to be unified: left and right.
 - Challenge: to find a binding for variables in these terms so that they are the same.

------
How do I unify the following terms?
```
a = b.
f(X, b) = f(a, Y).
f(X, b) = g(X, b).
a(X, X, b) = a(b, X, X).
a(X, X, b) = a(c, X, X).
a(X, f) = a(X, f).

pai(X, Y) = pai(ana, Y).
```
Which one is better? `{X->ana, Y->maria}, ou {X->ana}`?

* MGU = The most general unifier $\sigma$ is the MGU of T1=T2 iff, for any $\sigma'$ that unifies T1=T2, then $\sigma'$ is an instance of $\sigma$.

* Unification fills many roles:

------
What is role of unification below:
```
?- reverse([1,2,3], X).
X = [3, 2, 1].

?- X = 0.
X = 0.

?- [1,2,3] = [X|Y].
X = 1,
Y = [2, 3].
```

------
What is the result of `?- X = f(X)`? Or `append([], X, [a|X])`?

------
How to translate the following program to java?
```
p :- q, r.           boolean p() {return q() && r();}
q :- s.              boolean q() {return s();}
r :- s.              boolean r() {return s();}
s.                   boolean s() {return true;}
```

What about `p :- p.` ?
```java
boolean p() {return p();}
```

------
What about this program?
```
p :- q, r.           boolean p() {return q() && r();}
q :- s.              boolean q() {return s() || true;}
q.
r.                   boolean r() {return true;}
s :- 0 = 1.          boolean s() {return 0 == 1;}
```

* It is not always that simple: we may need substitutions...

------
How to prove `p(X)` given the program below:
```
p(f(Y)) :- q(Y), r(Y).
q(g(Z)).
q(h(Z)).
r(h(a)).
```

* Let's build an interpreter for prolog, using pseudo-code:

```
function resolution(clause, goals)
  let sub = the MGU of head(clause) and head(goals)
  return sub(tail(clause) concatenated with tail(goals))
```

------
What is `resolution([p(f(Y)), q(Y), r(Y)], [p(X), s(X)])`?
- `[q(Y), r(Y), s(f(Y))]`

```
function solve(goals)
  if goals is empty then succeed()
  else for each clause c in the program, in order
    if head(c) does not unify with head(goals) then do nothing
    else solve(resolution(c, goals))
```

------
What is `solve([p(X)])`?
```
                           sol([p(X)])
                               |
            sol(res([p(f(Y)), q(Y), r(Y)], [p(X)]))
                           {X = f(Y)}
                               |
                        sol([q(Y), r(Y)])
                      /                  \
sol(res([q(g(Z))], [q(Y), r(Y)]))        sol(res([q(h(Z))], [q(Y), r(Y)]))
               {Y = g(Z)}                           {Y = h(Z)}
                  |                                    |
           sol([r(g(Z))])                        sol([r(h(Z))])
                                                       |
                                          sol(res([r(h(a))], [r(h(Z))]))
                                                    {Z = a}
                                                       |
                                                    sol([])
```

* Proof trees:
  - two kinds of nodes: nothing's and solve's.
  - nothing nodes are leaves.
  - solve nodes are branches.
  - the root is a solve node containing the list of queries.

------
How to build the tree for `solve[p(X)]`?
```
                      {X->f(Y)}[q(Y), r(Y)]
                        /               \
{X->f(Y)}{Y->g(Z)}[r(g(Z))]     {X->f(Y)}{Y->h(Z)}[r(h(Z))]
                                         |
                                     {Z->a}[]
```

------
What is the result of consulting `p` in:
```
p :- p.
p.
```

What about:
```
p.
p :- p.
```

How is the proof tree of each of these programs?

* Sometimes variables must be substituted, to guarantee prolog's scope.

--------
Do you remember the `append` predicate?
```
([], L, L).
@([H|TA], B, [H|TC]) :- @(TA, B, TC).
```

--------
How is the proof tree of `solve(@(X, Y, [1, 2]))`.
```
                      @(X0, Y0, [1, 2])
                   /                    \
    {X0=[], Y0=B0, [1,2]=B0}        {X0=[H0|TA0], Y0=B1, [H0|TC0]=[1,2]}
                                    @(TA0, B1, [2])
                                 /                 \
          {TA0=[], B1=B2, [2]=B2}       {TA0=[H1|TA1], B1=B3, [H1|TC1]=[2]}
                                        @(TA1, B3, [])
                                              |
                                   {TA1=[], B3=B4, []=B4}
```

--------
Do you remmeber how reverse was defined?
```
myReverse([], []).
myReverse([H|T], X) :- myReverse(T, XAux), myAppend(XAux, [H], X).
```

--------
How is the proof tree of `solve([reverse([1,2], X)])`

```
                             rev([1, 2], X)
                            /
                           /
  {[1, 2] = [H0|T0, X = x0]}
  rev(T0, XAux1) = rev([2], XAux1)
  = rev([H1|T1], X1)@(XAux1, [H0], X)
  /
 /
{[2] = [H1|T1], X1 = XAux1}
   rev(T1, XAux2)@(XAux2, [H1], XAux1)
    /
   /
{T1 = [], XAux2 = []}
```

What about `solve([reverse(X, [1,2])])`?

* We can use write to debug:

```
reverse([], []).
reverse([H|T], X)
  :- reverse(T, XAux),
  write(H), write('|'), write(T), write(' '), write(XAux), write('\n'),
  append(XAux, [H], X).
```

In Prolog:
* A variable which is uninstantiated, i.e. no previous unifications were performed on it, can be unified with an atom, a term, or another uninstantiated variable, thus effectively becoming its alias. In many modern Prolog dialects and in first-order logic, a variable cannot be unified with a term that contains it; this is the so called occurs check.
* Two atoms can only be unified if they are identical.
* Similarly, a term can be unified with another term if the top function symbols and arities of the terms are identical and if the parameters can be unified simultaneously. Note that this is a recursive behavior.

In type theory, the analogous statements are:
* Any type variable unifies with any type expression, and is instantiated to that expression. A specific theory might restrict this rule with an occurs check.
* Two type constants unify only if they are the same type.
* Two type constructions unify only if they are applications of the same type constructor and all of their component types recursively unify.

## Reflection
```
/*
 Use this program by feeding the following clauses to the run-time system:
 ?- assert(connect(a, b)).
 ?- assert(connect(b, c)).
 ?- assert(connect(c, d)).
 ?- assert(connect(d, a)).
 ?- assert(at(a)).
*/
report :- write('You are at '), at(You), write(You), nl.

step(Dest) :-
  at(You),
  connect(You, Dest),
  retract(at(You)),
  assert(at(Dest)),
  write('You just left '), write(You), nl,
  write('Now you are at '), write(Dest), nl.

main :- write('\nNext move - '), read(Move), step(Move).
```
