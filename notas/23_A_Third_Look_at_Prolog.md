# A Third Look at Prolog

-----
How to implement a `myLength` function in Prolog?
```
myLength([], 0).
myLength([_|Tail], Len) :- myLength(Tail, TailLen), Len = TailLen + 1.
```

-----
What do we already know about prolog?

-----
What is the meaning of `1 + 2 * 3` in Java?  
What about Prolog?

- In Prolog we evaluate numeric expressions with 'is'.

-----
What is: `?- X = 1, Y is X + 2.`  
What about: `?- Y is X + 2, X = 1.`

- Evaluable predicates: `+, -, * , /, abs(Z), sqrt(Z)`.
- Ex.:  
`X is 1 /2. X is 1.0/2.0. X is 2/1. X is 2.0/1.0.`

- Numeric comparison: `<, >, =<, >=, =:=, =\=`

* Equality:
  - `X is Y` evaluates `Y` and unifies the result with `X`:  
     3 is 1+2 succeeds, but 1+2 is 3 fails
  - `X = Y` unifies `X` and `Y`, with no evaluation:  
     both 3 = 1+2 and 1+2 = 3 fail
  - `X =:= Y` evaluates both and compares:  
     both 3 =:= 1+2 and 1+2 =:= 3 succeed

Given that Prolog is dynamically typed, what is: `3 > a`?

* Any evaluated term must be fully instantiated.

-----
How to implement a myLength function?
```
myLength([], 0).
myLength([_|Tail], Len) :- myLength(Tail, TailLen), Len is TailLen + 1.
```

-----
What would happen if I change: `Len is TailLen + 1` to use `=, =:=,` or `==`?

-----
How to code sum?
```
sum([],0).
sum([Head|Tail],X) :- sum(Tail,TailSum), X is Head + TailSum.
```

How to make it a tail function?
```
sumAcc([], Acc, Acc).
sumAcc([H|T], Acc, X) :- Aux is Acc + H, sumAcc(T, Aux, X).
sumTC(L, S) :- sumAcc(L, 0, S).
```

What about:
```
sumAcc([], Acc, Acc).
sumAcc([H|T], Acc, X) :- sumAcc(T, Acc + H, X).
```

-----
Factorial?
```
fact(0, 1).
fact(1, 1).
fact(N, F) :- integer(N), N > 1, N1 is N - 1, fact(N1, F1), F is F1 * N.
```

- try also -
```
factorial(X, 1) :- X =:= 1.
factorial(X, Fact) :- X > 1, NewX is X - 1, factorial(NewX, NF), Fact is X * NF.
```

-----
How to test if a number is prime?
```
% P31 (**) Determine whether a given integer number is prime.

% is_prime(P) :- P is a prime number
% (integer) (+)

is_prime(2).
is_prime(3).
is_prime(P) :- integer(P), P > 3, P mod 2 =\= 0, \+ has_factor(P,3).  

% has_factor(N,L) :- N has an odd factor F >= L.
%    (integer, integer) (+,+)

has_factor(N,L) :- N mod L =:= 0.
has_factor(N,L) :- L * L < N, L2 is L + 2, has_factor(N,L2).
```

--------
Create a list containing all integers within a given range.
```
range(I,I,[I]).
range(I,K,[I|L]) :- I < K, I1 is I + 1, range(I1,K,L).
```

Find all the prime numbers from 1 to 10:
```
?- findall(N, (range(1, 10, L), member(N, L), is_prime(N)), L).
L = [2, 3, 5, 7].
```

--------
How to code a greatest common divisor?
```
gcd(X, Y, Z) :-
  X =:= Y, Z is X.
gcd(X, Y, Denom) :-
  X > Y,
  NewY is X - Y,
  gcd(Y, NewY, Denom).
gcd(X, Y, Denom) :-
  X < Y,
  gcd(Y, X, Denom).
```

Can you use Euclid's algorithm?
```
gcd(N1, N2, GCD) :- N1 < N2, euclid(N2, N1, GCD).
gcd(N1, N2, GCD) :- N2 > 0, N1 > N2, euclid(N1, N2, GCD).

euclid(N, 0, N).
euclid(N1, N2, GCD) :-
  N2 > 0, Q is N1 // N2, R is N1 - (N2 * Q), euclid(N2, R, GCD).
```

--------
Find all the subsets of a list:
```
subSet([], []).
subSet([H|T], [H|R]) :- subSet(T, R).
subSet([_|T], R) :- subSet(T, R).
```

--------
Solve the subset sum problem:
```
intSum(L, N, S) :- subSet(L, S), sumList(S, N).
```

--------
How to list all the solutions?
```
?- findall(S, intSum([2,3,5,7,11,13,17], 21, S), L).
```

--------
How to produce all the permutations of a list?
```
perm([], []).
perm(List, [H|Perm]) :- select(H, List, Rest), perm(Rest, Perm).
```

How to place them into a list?
```
?- findall(L, perm([a,b,c], L), Resp).
```

--------
We want to discover if a certain string represents an anagram of any English word. We have a dictionary dic.txt, with the known words, separated by a dot.

```
file_to_list(FILE, LIST) :-
  see(FILE), inquire([], LIST), seen.

inquire(IN, OUT):-
  read(Data), (Data == end_of_file -> OUT = IN;
  atom_codes(Data, LData), inquire([LData|IN], OUT) ) .

find_anagram(Dic, A, S) :-
  file_to_list(Dic, L), perm(A, X), member(X, L), name(S, X).
```

* Example:
```
?- find_anagram('dic.txt', "topa", S).
```

--------
Now we would like to invert the problem, and try to find all the words that can make up enigmas. That is, given a word W, we would like to see if it is possible to divide W into two halves, and see if the permutations of these two halves are present in the dictionary:

```
file_to_list(FILE, LIST) :-
  see(FILE), inquire([], LIST), seen.

inquire(IN, OUT):-
  read(Data), (Data == end_of_file -> OUT = IN;
  atom_codes(Data, LData), inquire([LData|IN], OUT) ) .

perm([], []).
perm(List, [H|Perm]) :- select(H, List, Rest), perm(Rest, Perm).

split([], [], []).
split([H|T], [H|T1], L) :- split(T, T1, L).
split([H|T], L, [H|T2]) :- split(T, L, T2).

comb2(L, L1, L2) :- split(L, L1, L2), length(L1, N), length(L2, N).

find_enigma(Dic, A, S1, S2) :-
  file_to_list(Dic, L), comb2(A, W1, W2), perm(W1, X1), member(X1, L), perm(W2, X2), member(X2, L), !, name(S1, X1), name(S2, X2).
```

--------
How to solve the eight-queens problem?

How to represent a queen?

How to test that a configuration is legal?
```
nocheck(_, []).
nocheck(X/Y, [X1/Y1 | Rest]) :-
  X =\= X1,
  Y =\= Y1,
  abs(Y1-Y) =\= abs(X1-X),
  nocheck(X/Y, Rest).
```

How to generate all the possible configurations?
```
legal([]).
legal([X/Y | Rest]) :-
  legal(Rest),
  member(X,[1,2,3,4,5,6,7,8]),
  member(Y,[1,2,3,4,5,6,7,8]),
  nocheck(X/Y, Rest).
```

Is `legal(X).` going to solve the problem?

How to get the solutions with eight queens?
```
X = [_,_,_,_,_,_,_,_], legal(X).
```

How to avoid trivial permutations?
```
  L = [1/_,2/_,3/_,4/_,5/_,6/_,7/_,8/_]
```

Can you improve the solution, given that we are assuming X in range, and
distinct?
```
nocheck(_, []).
nocheck(X/Y, [X1/Y1 | Rest]) :-
  Y =\= Y1,
  abs(Y1-Y) =\= abs(X1-X),
  nocheck(X/Y, Rest).

legal([]).
legal([X/Y | Rest]) :-
  legal(Rest),
  member(Y,[1,2,3,4,5,6,7,8]),
  nocheck(X/Y, Rest).

eightqueens(L) :-
  L = [1/_,2/_,3/_,4/_,5/_,6/_,7/_,8/_],legal(L)
```

What would you get with:
```
legal([]).
legal([X/Y | Rest]) :-
  legal(Rest),
  1 =< Y,
  Y =< 8,
  nocheck(X/Y, Rest).
```

And what about:
```
legal([]).
legal([X/Y | Rest]) :-
  member(Y,[1,2,3,4,5,6,7,8]),
  nocheck(X/Y, Rest),
  legal(Rest).
```

--------
How to find the number of solutions for the 8-queens problem?
```
findall(S, eightqueens(S), L), length(L, N).
```
