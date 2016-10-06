# The Lambda Calculus (Aula 09)

Pablo Diego Jose Francisco de Paula Juan Nepomuceno Maria de los Remedios
Cipriano de la Santisima Trinidad Ruiz y Picasso

1) Which language is more powerful, SML or C?

### The brief history of lambda-calculus.

* Lambda expressions:

```
<expr> ::= <name>
| \lambda <name> . <expr>
| <expr> <expr>
```

* The semantics of lambda-calculus is roughly defined by a simple substitution scheme:
$(\lambda v . E) E1 \rightarrow E$ [with each occurrence of $v$ replaced by E1]
This kind of reduction is called a betha reduction.

#### Example:
Identity = $\lambda x.x$  
$(\lambda x.x) y = y$

* We have also alpha reductions: we can change all the occurrences of a name
$n_1$ by another name $n_2$ in an expression $E$, given that $n_2$ does not already occur
in $E$: $\lambda x.x = \lambda z.z = \lambda y.y$

1.1) Do you remember the precedence rules in SML?

* Precedence:
Application associates to the left:  
E1 E2 E3 = ((E1 E2) E3)

1.1) What is the result of?  
$(\lambda x.\lambda y. x y) (\lambda z.z) w \\
  (\lambda y.(\lambda z.z) y) w \implies (\lambda z.z) w \implies w$

* Free and Bound variables:  
Given $\lambda x.E$, the occurrence of '$x$' in $E$ is considered bound.  
Example: $\lambda x.xy$  
  * Intuitively, a free variable is a variable that has not been declared.

* A substitution is only valid as long as it does not cause a free variable
to become bound:

1.2) Can you give an example of an unsafe substitution?  
$(\lambda y . (\lambda x . (\text{mul } y x))) x \implies \lambda x . \text{mul } x x$  
The original function is multiplication by $y$, and the second is a square.

Recursive definition:  
$<name>$ is free in $<name>$.  
$<name>$ is free in $\lambda <name1 > . <exp>$ if the identifier $<name> != <name1 >$  
and $<name>$ is free in $<exp>$.  
$<name>$ is free in $E1$ $E2$ if $<name>$ is free in $E1$ or it is free in $E2$.

* Substitution:  
Be careful not to mix up bound and free variables:

2) Solve $(\lambda x.(\lambda y.xy))y$  
We can do 'alpha-renaming'  
$(\lambda x.(\lambda t.xt))y \implies \lambda t.yt$

* A substitution should not change variables in the scope of other lambda  
declarations: $(\lambda x.x(\lambda x.x))y = y(\lambda y.y)$ is not wrong,
but it makes the program hard to read.  
or $(\lambda x . (\lambda x . x + 1) (x + x)) 2$  
- $\rightarrow (\lambda 2 . 2 + 1) (2 + 2) \rightarrow$ makes no sense to have $\lambda 2$  
- $\rightarrow (\lambda x . 2 + 1) (2 + 2) \rightarrow$ this is three, which is not probably what we want
- $\rightarrow (\lambda x . x + 1) (2 + 2) \rightarrow$ this is five, what is fine!

3) How to solve $(\lambda x. x(\lambda x.x))y$  
We can use alpha-renaming:  
$(\lambda x.x(\lambda x.x))y = (\lambda x.x(\lambda w.w))y = y(\lambda w.w)$

4) Solve $(\lambda x.(\lambda y.(x(\lambda x.xy))))y$  
renaming 1: $(\lambda x.(\lambda t.(x(\lambda x.xt))))y$  
renaming 2: $(\lambda x.(\lambda t.(x(\lambda u.ut))))y$  
$(\lambda t.(y(\lambda u.ut)))$

5) How to code a function Twice, that takes a function and a parameter,
and applies the function on the parameter twice?  
$\lambda f.\lambda p.f(fp)$

6) How to represent numbers using lambda-expressions?
- Everything is a convention!  
A number is a function that takes a function $s$ plus a constant $z$. The
number $N$ is formed by applying s $N$ times on $z$.  
$Zero = \lambda s.\lambda z.z\\
One = \lambda s.\lambda z.(sz)\\
Two = \lambda s.\lambda z.s(sz)\\
Three = \lambda s.\lambda z.s(s(sz))\\
\dots$

7) How to define the successor function?  
$Succ = \lambda n.\lambda y.\lambda x.y(nyx)$

Example 1 - successor applied to zero:  
  $(\lambda n.\lambda y.\lambda x.y(nyx))(\lambda s.\lambda z.z) =\\
  \lambda y.\lambda x.y((\lambda s.\lambda z.z)yx) =\\
  \lambda y.\lambda x.yx$

Example 2 - successor applied to one:  
  $(\lambda n.\lambda y.\lambda x.y(nyx))(\lambda s.\lambda z.sz) =\\
  \lambda y.\lambda x.y((\lambda s.\lambda z.sz)yx) =\\
  \lambda y.\lambda x.y(yx)$

8) How to define addition of $n$ and $m$?  
What about $ADD = \lambda m.\lambda n.\lambda x.\lambda y.m x (n x y)$  
ex.:  
$ADD\;2\; 3 =\\
 (\lambda m.\lambda n.\lambda x.\lambda y.m x (n x y))\;2\;3 =\\
 \lambda x.\lambda y.2 x (3 x y) =\\
 \lambda x.\lambda y.2 x (x (x (x y))) =\\
 \lambda x.\lambda y.(\lambda s.\lambda z.s (s z)) x (x (x (x y))) =\\
 \lambda x.\lambda y.x (x (x (x (x y))))$

8.1) Can I use the successor function?
- Apply the successor function $n$ times on $m$, e.g: $\text{ADD} = \lambda m . \lambda n . m \text{ SUCC } n$  
Example:  
  $2 + 3 =\\
  \text{Two Succ Three} =\\
  (\lambda s.\lambda z.s(sz)) \text{ SUCC Three} =\\
  (\lambda z. \text{ SUCC SUCC } z) \text{ Three} =\\
  \text{ SUCC} (\text{ SUCC Three}) =\\
  \text{ SUCC} (\text{ Four}) = \text{ Five}$

9) How to implement multiplication?  
$\text{MUL} = \lambda n_1.\lambda n_2.\lambda s. n_1 (n_2 s)$

  9.1) How to understant this query? What would happen once we pass a number $n_1$
as parameter?
$\lambda n_1 . \lambda n_2 . \lambda s . n_1 (n_2 s) (\lambda a . \lambda b . a(a(...(a(ab)))))\\
\lambda n_2 . \lambda s . (\lambda a . \lambda b . a(a(...(a(ab))))) (n_2 s)\\
\lambda n_2 . \lambda s . (\lambda b . (n_2 s) ( (n_2 s) (...( (n_2 s) ( (n_2 s) b))...)))$

9.2) What would happen if I pass another number $n2$ as parameter?  
$\lambda s . (\lambda b . (n_2 s) ( (n_2 s) (...( (n_2 s) (s(s...(s b)...)))...)))\\
\lambda s . (\lambda b . (n_2 s) ( (n_2 s) (...( (s(s... s(s(s...(s b)...)...))))...)))\\
\lambda s . (\lambda b . (s(s(s(s(s...(s(sb))))))))$  
ex.:  
$\text{MUL } 2\;3 =\\
\lambda s . 2 (3 s)=\\
\lambda s . 2 ((\lambda a . \lambda b . a(a(a b))) s)=\\
\lambda s . 2 (\lambda b . s(s(s b)))=\\
\lambda s . (\lambda c . \lambda d . c(c d)) (\lambda b . s(s(s b)))=\\
\lambda s . (\lambda d . (\lambda b . s(s(s b))) (((\lambda b . s(s(s b)))) d))=\\
\lambda s . (\lambda d . (\lambda b . s(s(s b))) (s(s(s d))) )=\\
\lambda s . (\lambda d . s(s(s(s(s(s d))))))$

10) How to define conditionals:  
$T = \lambda x.\lambda y.x\\
F = \lambda x.\lambda y.y$

11) How to define AND, OR and NOT?  
$AND = \lambda x.\lambda y.xyF\\
OR = \lambda x.\lambda y.xTy\\
NOT = \lambda x.xFT$

Ex.:

11.1) $AND\;\;T\;F =\\
(\lambda x.\lambda y.xyF) T F =\\
T F F = (\lambda x.\lambda y.x) F F = F$

11.2) $OR\;\;F\;T =\\
(\lambda x.\lambda y.xTy) F T =\\
T T F = (\lambda x.\lambda y.x) T F = T$

11.3) $NOT\;\;T =\\
(\lambda x.xFT) T =\\
T F T = (\lambda x.\lambda y.x) F T = F$

12) How to define a conditional?  
$IF = \lambda c . \lambda e_1 . \lambda e_2 . c e_1 e_2$

12) How to define a function that is true if the argument is the number
zero, and false otherwise?  
$Z = \lambda x.x F\;NOT\;F$

12.1) What is $ZERO\;F\;NOT\;F?$  ;; remember, application associates to the left:  
$(\lambda s. \lambda z . z) F\;NOT\;F =\\
(\lambda z . z)\;NOT\;F =\\
NOT\;F = T$

12.2) What is $n\;F\;NOT\;F$, $n > ZERO$?
;; $F$ takes two args, and returns the second.  
$(F\;(F\;(F\;... (F\;NOT)))) F\;=\\
((\lambda x.\lambda y.y) (F\;(F\;... (F\;NOT)))) F\;=\\
(\lambda y.y) F = F$

Ex.:

12.3) Evaluate $Z Zero =$  
$(\lambda x.x F\;NOT\;F) Zero =\\
Zero F\;NOT\;F =\\
(\lambda s.\lambda z.z) F\;NOT\;F =\\
NOT\;F = T$

12.4) Evaluate $Z \text{ Three} =$  
$(\lambda x.x F\;NOT\;F) \text{Three} =\\
\text{Three } F\;NOT\;F =\\
(F (F (F\;NOT))) F =\\
((\lambda x.\lambda y.y) (F (F\;NOT))) F =\\
(\lambda y.y) F = F$
