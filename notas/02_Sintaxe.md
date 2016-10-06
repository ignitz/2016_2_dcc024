# Sintaxe (Aula 02)

-----------------------------------------------------

Administrative stuff:  
1) Did everybody got my e-mail?
2) Are there questions from the last class?
   - One point that may not have been made clear is that functional and  imperative are not absolute paradigms. Most of the languages provide
   characteristics of both, e.g ML has references...
3) Did anybody get into Euler's?
4) Did anybody get the course slides?
5) Did anybody read the material for today's class?

---------------------------------------

0) The sentense "I jump" makes perfect sense and sounds good to me
   The sentense "The chair jumps" does not make sense, but sounds good
   The sentence "I of" does not make sense, nor sounds good, but I can read it
   The sentence "oIf" is unreadable
   The sentence "o#u@f" is unspellable
   What is the difference in each case?

1) How I would describe a programming language?
   - syntax
   - semantics

2) Has anybody heard of Noam Chomsky? What about John Backus?
> Noam Chomsky é o linguista cabuloso da regra gramatical.
> John Warner Backus (Filadélfia, 31 de dezembro de 1924 — Ashland, 17 de março de 2007) foi um cientista da computação estadunidense. Conhecido por criar a primeira linguagem de programação de alto nível - o Fortran -, a notação BNF e o conceito de programação em nível de funções.
> Recebeu em 1977 o Prêmio Turing, por suas contribuições para o desenvolvimento de sistemas de programação de alto nível, principalmente por seu trabalho no Fortran e por publicações sobre métodos formais na especificação de linguagens de programação.

Grammar is ``<T, N, P, S>``
- ``T`` is a set of tokens,
- ``N`` is a set of nonterminals,
- ``P`` is a set of productions,
- ``S`` is a start symbol.


- Tokens form the vocabulary of the language;
- Nonterminals are not part of the language  they are enclosed in brackets ``<NP>``;
- Production is ``<non-terminal> := <expr1> ... <exprn>``
Productions may have '|' to separate alternatives.
- The start symbol forms the root of any parse tree for the grammar.

2.1. Have you ever seen the grammar of a true programming language?

Example:
```
<exp>   ::= <exp> "+" <exp>
         |  <exp> "*" <exp>
         |  "(" <exp> ")"
         |  "a"
         |  "b"
         |  "c"
```

Parsing is the problem of building parsing trees for strings.

3) Example: show parse trees for each of these strings:
```
  a + b
  a * b + c
  (a + b)
  (a + (b))
```

We will be only dealing with Context-free grammars: the left side must be a
single non-terminal.

4) How would be a grammar for the assembly language that we saw last class?
```
P ::= I
   |  I P
I ::= mov A A
   |  add A A
   |  store A A
   |  load A A
   |  jump A
A ::= $num
   | eax, ebx, ecx, edx, ebp, esi, edi, esp
```

5) Try to come up with a grammar for Portuguese:

Example:
```
<sentenca>     ::= <expr_nominal> <predicado>
<expr_nominal> ::= <artigo> <nome>                  ''o menino
                |  <artigo> <nome> <expr_prepos>    ''o filho de Maria
<predicado>    ::= <verbo>                          ''caminha
                |  <verbo> <expr_nominal>           ''eh bonito
                |  <verbo> <expr_nominal> <expr_prepos>      ''gosta de Maria
<expr_prepos>  ::= <preposicao> <expr_nominal>
<nome>         ::= "menino" | "menina" | "pato" | "telescopio" | "musica"
                | "pena"
<preposicao>   ::= "com" | "ate"
<verbo>        ::= "viu" | "esta" | "e" | "canta" | "surpreende" | "toca"
<artigo>       ::= "um" | "uma" | "o" | "a"
```

A grammar is ambiguous if some phrase in the language gener-
ated by the grammar has two distinct derivation trees.

6) Can you find any ambiguity in the original grammar?
```
<exp>   ::= <exp> "+" <exp>
         |  <exp> "*" <exp>
         |  "(" <exp> ")"
         |  "a" | "b" | "c"
```

Ex.: A menina toca o pato com a pena.
Does pena belongs to pato, or is it the tool used by menina?

7) How can this grammar produce an infinite string?

#### Ambiguity:

8) Does anybody see the ambiguity in this grammar?
```
<cmd> ::= if <bool_expr> then <cmd>
       |  if <bool_expr> then <cmd> else <cmd>
```

How to read:
```
if (a > b) then
 if (c > d) then
   print(1)
 else
   print(2)
```

--------------------------------------------------------------------------------

We recognize languages using programs called parsers.
Prolog is particularly good for writing parsers.

9) Write the parser for the Portuguese grammar in Prolog.
   - portug.pl

10) Show that ``a,menina,toca,o,pato,com,a,pena`` has two derivations, whereas
``a,menina,toca,o,pato`` has only one.

11) Play a little bit with the grammar, specifying different variables in some
productions, e.g:
```
sentenca([a,Sujeito,toca,o,pato],[]).

sentenca --> expr_nominal, predicado.

expr_nominal --> artigo, nome.
expr_nominal --> artigo, nome, expr_preposicional.

predicado --> verbo.
predicado --> verbo, expr_nominal.
predicado --> verbo, expr_nominal, expr_preposicional.

expr_preposicional --> preposicao, expr_nominal.

nome --> [menino]; [menina]; [pato]; [telescopio]; [musica]; [pena].

preposicao --> [ate]; [com].

verbo --> [e]; [viu]; [esta]; [toca]; [canta]; [surpreende].

artigo --> [a]; [o]; [uma]; [um].
```

--------------------------------------------------------------------------------

12) Let's write a grammar for numbers in Prolog?
    - number.pl

```
<number> ::= <digit> <number>
          |  <digit>
<digit>  ::= "0", "1", "2", "3", "4", "5", "6", "7", "8", "9"
```

In prolog, this grammar looks like:

```
number --> digit, number.
number --> digit.

digit --> [0] ; [1] ; [2] ; [3] ; [4] ; [5] ; [6] ; [7] ; [8] ; [9].
```
In prolog,
   ',' means AND, and ';' means OR.
   non-terminals are enclosed in []'s.

13) How to add signal to these numbers?
E.g: ``sigNum([-, 3, 4, 5], [])``.

```
sigNum --> signal, number.
sigNum --> number.
signal --> [-]; [+].
```

14) How to add a decimal dot to the signed numbers?
  E.g: ``decNum([-, 2, 3, 4, ., 4, 5], [])``

```
decNum --> sigNum, dot, number.
decNum --> sigNum.
dot --> [.].
```

--------------------------------------------------------------------------------

If every production of a grammar is in the form
``<p> ::= t <p'>, or <p> ::= t, or <p> ::= \epsilon``, then the grammar is said to
be regular. Tokens are normally described by regular grammars.

Phrasal structure
Lexical structure
A compiler: scanner and parser.

--------------------------------------------------------------------------------
There is a hierarchy of grammars.

15) Example:
Could you come up with a grammar to recognize strings having equal
numbers of a's, b's, and c's in that order? Namely, the set  ``{ abc, aabbcc, aaabbbccc, ... }``?

- regular grammars.
  - Finite state automaton.
- context-free grammars.
  - Non-deterministic Pushdown automaton.
- context-sensitive grammars.
  - Linear bounded automaton: Non-deterministic Turing machine with bounded tape
- Unrestricted grammars.
  - Turing machine
