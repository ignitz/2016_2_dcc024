# Introduction to Programming in Python

### Let's review the characteristics of imperative languages:

What are the main differences between imperative and functional languages?

---------------------

Normally we say that a functional language has expressions, and an imperative language has commands and expressions. What is a command?

There are three major types of commands:
- Branches
  - Conditionals
  - Iterations
- Sequences
  - Blocks
- Assignments

---------------------

Perhaps the archetype imperative command is the assignment.  
We have not seen assignments in SML, yet, we have seen programs such as
```
- let val a = 2 val b = 3 in a + b end;
```
Why is '`val a = 2`' not an assignment

What is then the semantics of an assignment?

The issue of portability.
A little of the history of Python.

The popularity of Python:
http://www.tiobe.com/index.php/content/paperinfo/tpci/index.html

The guiding principles behind python:
>>> import this

---------------------

What is the meaning of 'Explicit is better than implicit.'

Python is a dynamic language. What is this?
- heavy use of the heap

This is not an exclusive characteristic of dynamic languages. Which
other languages use the heap heavily?

- dynamic typing
```python
>>> if 2 != 3:
...   a = "fernando"
... else:
...   a = 0
```

How could you write a similar program in C, SML or Java?
```python
- eval and exec
a = 1
exec 'a = a + 1'
print a
```

Notice that you are changing a variable in your scope.
How could you write a similar program in C, SML or Java?

---------------------

Python programs are interpreted or virtualized. What does this mean?

---------------------

Normally these programs are parsed, transformed in some sort of
intermediate representation, and then executed in a virtual machine.

The programs may be JIT compiled during execution.

- What is a VM?
- What is an intermediate representation?
- What is JIT compilation?

---------------------
How to execute python programs?
- Interactive interpreter:
```python
$> python
>>> 2 + 3.0
5.0
```

- Loading a file:
```python
$> echo "print \"Hello, world\n\"" > hello.py
$> python hello.py
```

- As an executable script
```python
$> cat script.py
#! /usr/bin/env python
print "Hello, world!"
$> chmod u+x script.py
$> ./script.py
```

Types in python are verified during the program execution; however, the
types do exist!

---------------------
Python has some primitive types. What are primitive types?

- What are the primitive types in SML?
- Which primitive types we are likely to find in Python?

- integer numbers:
```python
>>> (50-5*6)/4
5
```

- floating point numbers:
```python
>>> (50 - 5.0 * 6) / 4
5.0
```

- Strings:
```python
>>> s = "Fernando" + " Magno"
>>> print s
Fernando Magno
```

Some people would say that string is not really a primitive type.
Yet, everybody agrees that string is a "built in" type.
Explain the difference between primitive and built in.

- Booleans
```python
>>> 3 > 2
True
```

---------------------
What are the operators of the language?
```
* / %
+ -
<< >>
< > <= >=
== !=
~
&
^
|
not
and
or
= += -= *= /= %= &= ^= |= <<= >>=
```

---------------------
Compared to C, what are we really missing?
```
exp++ exp--
++exp --exp +exp -exp
```

Does python have conditional expressions, like ternary comparisons in C?
```python
v = 1 if 0 else "oi"
v = 1 if False else "oi"
v = 1 if None else "oi"
```

---------------------
Comparisons can be chained, e.g `a < b == c`.  
How to code a function `inbetween(a, b, c)`?
```python
>>> def inbetween(a, b, c): return a <= b <= c
...
>>> inbetween(2, 3, 3)
True
>>> inbetween(2, 3, 2)
False
```

---------------------
We see many assignments. An assignment has a left side and a right
side. What can exist on the right side?

- What about the left side?

- What is the meaning of
```python
>>> x = y = z = 0
```

---------------------
What are compound data types? Which compound data types are we likely
to find in Python?

- Lists:
```python
>>> a = ['spam', 'eggs', 100, 1234]
>>> a
['spam', 'eggs', 100, 1234]
>>> a[0]
'spam'
>>> a[3]
1234
>>> a[-2]
100
>>> a[1:-1]
['eggs', 100]
>>> a[:2] + ['bacon', 2*2]
['spam', 'eggs', 'bacon', 4]
>>> 3*a[:3] + ['Boe!']
['spam', 'eggs', 100, 'spam', 'eggs', 100, 'spam', 'eggs', 100, 'Boe!']
```

---------------------
Strings have some list operations too, but strings are not lists.  
Why? E.g:

```python
>>> a = "dcc024"
>>> a[0]
'd'
>>> a[-1]
'4'
>>> a[1:-1]
'cc02'
>>> a[0] = 'x'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
```

- It is possible to do a lot of things with list slices:
```python
>>> # Replace some items:
... a[0:2] = [1, 12]
>>> a
[1, 12, 123, 1234]
>>> # Remove some:
... a[0:2] = []
>>> a
[123, 1234]
>>> # Insert some:
... a[1:1] = ['bletch', 'xyzzy']
>>> a
[123, 'bletch', 'xyzzy', 1234]
>>> a[:0] = a # Insert (a copy of) itself at the beginning
>>> a
[123, 'bletch', 'xyzzy', 1234, 123, 'bletch', 'xyzzy', 1234]
```

---------------------
How to code a function duplist that receives a list l, and updates l,
so that the new list is l + l?
```python
def duplist0(l): l[:0] = l
```

How to do it without side effects?
```python
def duplist1(l): return l + l
```

---------------------
Ok, we have seen lists, what else are we missing from SML?
```python
>>> t = 12345, 54321, 'hello!'
>>> t[0]
12345
>>> t
(12345, 54321, 'hello!')
>>> # Tuples may be nested:
... u = t, (1, 2, 3, 4, 5)
>>> u
((12345, 54321, 'hello!'), (1, 2, 3, 4, 5))
```

---------------------
How to create a tuple with 0 or 1 items?
```python
>>> empty = ()
>>> singleton = 'hello', # <-- note trailing comma
>>> len(empty)
0
>>> len(singleton)
1
>>> singleton
('hello',)
```

- We can also do some sort of pattern matching in tuples:
```python
>>> x, y, z = t
```

---------------------
Finally, we also have dictionaries as built in compound types. What
is a dictionary?
```python
>>> tel = {'jack': 4098, 'sape': 4139}
>>> tel['guido'] = 4127
>>> tel
{'sape': 4139, 'guido': 4127, 'jack': 4098}
>>> tel['jack']
4098
>>> del tel['sape']
>>> tel['guido'] = 4127
>>> tel
{'guido': 4127, 'irv': 4127, 'jack': 4098}
>>> tel.keys()
['guido', 'irv', 'jack']
>>> tel.has_key('guido')
True
>>> knights = {'gallahad': 'the pure', 'robin': 'the brave'}
>>> for k, v in knights.iteritems():
... print k, v
...
gallahad the pure
robin the brave
```

---------------------
The main compilable unit in Python is the function:
def sum(a, b)
  return a + b

---------------------
What was the main "control flow structure" that we have used in SML?

Imperative languages provide many control flow structures. Examples?
- if/elif/else:

---------------------
How to code a recursive factorial function?
```python
def fact(n):
  if n > 1:
    return n * fact (n - 1)
  elif n == 1 or n == 0:
    return 1
  else:
    raise ArithmeticError("Factorial needs positive integers!")

print fact(1)
print fact(-1)
```

---------------------
How to go over the elements of a list?

- for:
```python
... a = ['cat', 'window', 'defenestrate']
>>> for x in a:
...   print x, len(x)
...
```
- for + range:
```python
>>> a = ['Mary', 'had', 'a', 'little', 'lamb']
>>> for i in range(len(a)):
...   print i, a[i]
...
```

---------------------
What does the program below do?
- break and else inside loops:
```python
def mystery(n):
  for x in range(2, n):
    if n % x == 0:
      print n, 'equals ', x, ' * ', n/x
      break
  else:
    print n, ' passes the test!'
```

How to implement a factorial using a while loop?
- while:
```python
def factW(n):
  ans = 1
  while n > 1:
    ans *= n
    n -= 1
  return ans
```

What would happened if I tried:
```python
def factW(n):
  ans = 1
  while n > 1:
    ans *= n
  n -= 1 // moved this statement two spaces left
  return ans
```

-----------------
What are the advantages and disadvantages of adding block semantics to
indentation?

- How to leave two commands in the same line?
```python
def factW(n):
  ans = 1
  while n > 1:
    ans *= n ; n -= 1
  return ans
```

-------------------
Do you remember what are first class values?

How to know if functions in python are first class values?
- Functions in Python are first class values:
```python
>>> f = factW
>>> f(7)
5040
```

------------------
Do you remember what are high-order functions?

- How to know if python has high-order functions?
```python
def computeFacts(f, l):
  for x in l:
    print x, f(x)
```

------------------
Do you remember what are closures?

- How to code something like $\lambda n . \lambda x . x + n$?
```python
def make_incrementor(n):
  def inc(x):
    return x + n
  return inc
```

- or -

```python
>>> def make_incrementor(n):
... return lambda x: x + n
...
>>> f = make_incrementor(42)
>>> f(0)
42
```

- or -

```python
>>> f = lambda x : lambda y : x + y
>>> a = f(3)
>>> a(2)
5
```

-------------------
Have you ever heard about list comprehension?
```python
>>> [3*x for x in vec]
[6, 12, 18]
>>> [3*x for x in vec if x > 3]
[12, 18]
```

Code a function that returns the strings that start with 'a' in a
list of strings:
```python
def filterC(c, l):
  return [s for s in l if s[0] == c]
>>> filterC('a', ['abacate', 'uva', 'ameixa', 'banana', 'abacaxi'])
['abacate', 'ameixa', 'abacaxi']
```

### Map, reduce
How to code, in SML, a function that maps the elements in a list of
integers to their cubes?

How to do the same in Python?
```python
map(lambda n: n * n * n, range(1, 11))
```

-----------------
How to code a function that sums up the elements in a list, in SML?

How to do the same in Python?
```python
>>> def sum(l): return reduce(lambda a, b: a + b, l, 0)
...
>>> sum([1,2,3,4])
10
```

----------------
It is very important to be able to check the type of a variable at
runtime. Why this is particularly important in Python?

- How to do it?
```python
>>> a = None
>>> type(a)
<type 'NoneType'>
>>> a = 'a'
>>> type(a)
<type 'str'>
>>> a = ['a', 0]
>>> type(a)
<type 'list'>
```

----------------
Write a function that divides two numbers, n and d. This function
should return the special element None in case `d == 0`:
```python
def div(n, d):
  if d == 0:
    return None
  else:
    return n/d
```

-----------------
Now, write a function that uses `div`, and check if the answer is `None`:

```python
def testDiv():
  while True:
    n = float(raw_input("Please, enter the dividend: "))
    d = float(raw_input("Please, enter the divisor: "))
    a = div(n, d)
    if type(a) == type(None):
      break
    else:
      print a
```

----------------
How to discover if two strings are anagrams?
```python
def anagrams(s1, s2):
  if (len(s1) != len(s2)):
    return False
  else:
    table = {}
    for c in s1:
      if table.has_key(c):
        table[c] += 1
      else:
        table[c] = 1
    for c in s2:
      try:
        table[c] -= 1
        if table[c] == 0:
          table.pop(c)
      except KeyError:
        return False
    return True
```
