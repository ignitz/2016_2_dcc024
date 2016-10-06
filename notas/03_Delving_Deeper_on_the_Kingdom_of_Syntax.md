# Delving Deeper on the Kingdom of Syntax (Aula 03)

Administrative stuff:

1) Before sending me code for the project Euler, what makes a good program?
  - Indent the code.
  - Be consistent with formating: use only tabs (or spaces) for indentation.
  - Be consistent with naming conventions: bean_count, BeanCount, beanCoun, etc.
  - Use good, informative names.
  - Add comments (sensible comments).
    - Header file comments: purpose of file, author, date, etc
    - Function comments: purpose of function, usage example
    - Line comments: why it is done in that way.

2) Has anyone taken a look into the slides?

3) Review of last class:
  - How I would describe a programming language?
    - syntax ``<T, N, P, S>``
    - semantics

--------------------------------------------------------------------------------

1) What is the role of grammars in compilers?

```
scanner(a) --> parser(b) --> optimizer(c) --> code generator(d)
a: separates strings into tokens
b: builds syntax tree
c: optimizes the program
d: generates machine code
```

3) Grammars don't define the whole meaning of the language, but they define
some meaning. Which meaning is defined by grammars?
- Associativity
- precedence

4) What is an operator?
- How many types of operators are there?
  - unary
  - binary
  - ternary
- What defines associativity and precedence of operators?
  - the structure of the parsing tree:
- What is the meaning of this expression in C?
``a = b < c ? * p + b * c : 1 << d ()``

- Can you build a parsing tree for this expression?

```
       ___ass___
     /     |     \
    /      |      \
  a        =    ____ternary_____
              _/       |        \_
             /         |          \
            cp        add          sft
          / | \     /  |  \      /  |  \
         b  <  c   pt  +   mul  1  <<   func
                  / \     / | \          / \
                 *   p   b  *  c        d  ()
```

5) What is the value of ``2 + 3 * 4`` in the grammar below?
```
<exp> ::= <exp> + <exp>
       | <exp> * <exp>
       | (<exp>)
       | <number>
```

We can evaluate the parse tree to get the value of the arithmetic expression.

- How to evaluate a parse tree?
- What is the precedence of addition and multiplication in the grammar above?
- How to modify this grammar so that multiplication has higher precedence?
```
<exp> ::= <exp> + <exp> | <mulexp>
<mulexp> ::= <mulexp> * <mulexp>
           | (<exp>)
           | <number>
```

6) What is the associativity of '+'? Does it associate to the left, or to the
   right?

- What does it mean to say that something associates to the left or right?

- Can anyone give an example of an operator that is right associative in C?
  - assignment.

- An example where associativity could change the meaning of the
expression?
``1 - 2 - 3``

- How to modify the grammar, so that the operations associate to the right?
```
<exp> ::= <mulexp> + <exp>
       | <mulexp>
<mulexp> ::= <rootexp> * <mulexp>
          | <rootexp>
<rootexp> ::= ( <exp> )
           | <number>
```

- What if I want them to associate to the left?

- How to write this grammar in Prolog?
```
/* num0.pl */
expr --> mulexp, [+], expr.
expr --> mulexp.
mulexp --> rootexp, [*], mulexp.
mulexp --> rootexp.
rootexp --> ['('], expr, [')'].
rootexp --> number.
number --> digit.
number --> digit, number.
digit --> [0] ; [1] ; [2] ; [3] ; [4] ; [5] ; [6] ; [7] ; [8] ; [9].
```

- Is this grammar right or left associative?

7) Starting with this grammar:
```
<exp> ::= <mulexp> + <exp>
       | <mulexp>
<mulexp> ::= <rootexp> * <mulexp>
          | <rootexp>
<rootexp> ::= ( <exp> )
           | <number>
```

- Add a right-associative `&` operator, at lower precedence than any of the
others, e.g:
``expr([2,3,+,4,5,&,1,2,*,3,4], []).``

- Then add a right-associative unary `**` operator, at higher
precedence than any of the others, e.g:
``expr([2,3,+,4,5,&,3,4,*,**,3,4], []).``

```
/* num1.pl */
expr --> ampexp, [&], expr.
expr --> ampexp.
ampexp --> mulexp, [+], expr.
ampexp --> mulexp.
mulexp --> rootexp, [*], mulexp.
mulexp --> rootexp.
rootexp --> ['('], expr, [')'].
rootexp --> [**], rootexp.
rootexp --> number.
number --> digit.
number --> digit, number.
digit --> [0] ; [1] ; [2] ; [3] ; [4] ; [5] ; [6] ; [7] ; [8] ; [9].
```
Ex.:  `expr([**,1,2,*,'(',1,+,2,')'],[]).`

8) Let's revisit our Portuguese grammar. Do you remember how was it?
```
sentenca --> expr_nominal, predicado.
expr_nominal --> artigo, nome.
expr_nominal --> artigo, nome, expr_preposicional.
predicado --> verbo.
predicado --> verbo, expr_nominal.
predicado --> verbo, expr_nominal, expr_preposicional.
expr_preposicional --> preposicao, expr_nominal.
nome --> [menino] ; [menina] ; [pato] ; [telescopio] ; [musica] ; [pena].
preposicao --> [ate] ; [com].
verbo --> [e] ; [viu] ; [esta] ; [toca] ; [conta] ; [surpreende].
artigo --> [o] ; [a] ; [um] ; [uma].
```

9) We can embed computation in the grammar clauses. Can you code a grammar that
counts the words in the sentences?
```
sentenca(W) --> expr_nominal(W1), predicado(W2), {W is W1 + W2}.
expr_nominal(W) --> artigo, nome, {W is 2}.
expr_nominal(W) --> artigo, nome, expr_preposicional(WP), {W is WP + 2}.
predicado(W) --> verbo, {W is 1}.
predicado(W) --> verbo, expr_nominal(WN), {W is WN + 1}.
predicado(W) --> verbo, expr_nominal(WN), expr_preposicional(WP), {W is 1 + WN + WP}.
expr_preposicional(W) --> preposicao, expr_nominal(WN), {W is 1 + WN}.
nome --> [menino], [menina], [pato], [telescopio], [musica], [pena].
preposicao --> [ate], [com].
verbo --> [e], [viu], [esta], [toca], [conta], [surpreende].
artigo --> [o], [a], [um], [uma].
```

Example: `sentenca(W, [a, menina, toca, o, pato], []).`

`W = 5 ? ;`

10) Can you use a similar technique to develop an interpreter to our grammar
of arithmetic expressions?
```
expr(N) --> mulexp(N1), [+], expr(N2), {N is N1 + N2}.
expr(N) --> mulexp(N).
mulexp(N) --> rootexp(N1), [*], mulexp(N2), {N is N1 * N2}.
mulexp(N) --> rootexp(N).
rootexp(N) --> ['('], expr(N), [')'].
rootexp(N) --> number(N, _).
number(N, 1) --> digit(N).
number(N, C) --> digit(ND), number(NN, C1), {
  C is C1 * 10,
  N is ND * C + NN
}.
digit(N) --> [0], {N is 0}
           ; [1], {N is 1}
           ; [2], {N is 2}
           ; [3], {N is 3}
           ; [4], {N is 4}
           ; [5], {N is 5}
           ; [6], {N is 6}
           ; [7], {N is 7}
           ; [8], {N is 8}
           ; [9], {N is 9}.
```
Ex.:  `expr(N, [2,*,'(',3,+,4,')'], []).`

11) Normally we can encode precedence and associativity as a tree. Let's call
this tree the "abstract syntax tree". What is the tree for this expression:
`10 * (15 + 2)?`
- The meaning of the parenthesis is implicit in the structure of the tree.
- A compiler does not have to represent the whole tree internally:
```
        *
      /   \
     10    +
         /   \
       15     2
```
11) Can you create an abstract syntax tree for the portuguese grammar?
```
sentenca(sTN(E,P)) --> expr_nominal(E), predicado(P).
expr_nominal(eNL(A,N)) --> artigo(A), nome(N).
expr_nominal(eNL(A,N,EP)) --> artigo(A), nome(N), expr_preposicional(EP).
predicado(pRD(V)) --> verbo(V).
predicado(pRD(V,EN)) --> verbo(V), expr_nominal(EN).
predicado(pRD(V,EN,EP)) --> verbo(V), expr_nominal(EN), expr_preposicional(EP).
expr_preposicional(ePP(P,EN)) --> preposicao(P), expr_nominal(EN).
nome(nOM) --> [menino] ; [menina] ; [pato] ; [telescopio] ; [musica] ; [pena].
preposicao(pRP) --> [ate] ; [com].
verbo(vRB) --> [e] ; [viu] ; [esta] ; [toca] ; [conta] ; [surpreende].
artigo(aRT) --> [o] ; [a] ; [um] ; [uma].
```
Example: `sentenca(T, [a, menina, toca, o, pato, com, a, pena], []).`

```
T = sTN(eNL(aRT,nOM),pRD(vRB,eNL(aRT,nOM,ePP(pRP,eNL(aRT,nOM))))) ? ;

T = sTN(eNL(aRT,nOM),pRD(vRB,eNL(aRT,nOM),ePP(pRP,eNL(aRT,nOM)))) ? ;
```

- This AST has a problem, the leaf nodes have no names. How to add the
names?

- What are the two syntaxe trees produced for:
`sentenca(W, T, [a, menina, toca, o, pato, com, a, pena], []).`

12) Is the grammar $a^n b^n$ regular?

13) Specify a prolog grammar that recognizes this language.
```
/* ab.pl */

ab --> [a] , ab , [b].
ab --> [].
```

14) Specify a prolog grammar that recognizes this $a^n b^n c^n$:
```
/* abc.pl */

abc --> as(N), bs(N), cs(N).

as(0) --> [].
as(M) --> [a], as(N), {M is N + 1}.

bs(0) --> [].
bs(M) --> [b], bs(N), {M is N + 1}.

cs(0) --> [].
cs(M) --> [c], cs(N), {M is N + 1}.
```

15) Specify a prolog grammar that recognizes $a^n b^m a^n b^m$
```
abab --> an(N), bm(M), an(N), bm(M).

an(0) --> [].
an(NN) --> [a], an(N), {NN is N + 1}.

bm(0) --> [].
bm(MM) --> [b], bm(M), {MM is M + 1}.
```

16) The compiler/interpreter uses the parsing tree for many things. Examples?
- Syntactic verification;
- Type checking;
- Interpretation.
- Code generation.
- Program analysis.

17) Which kinds of static verifications can a compiler do on an input program?

- Which of them can be done directly via context-free grammars?

- Which cannot?
