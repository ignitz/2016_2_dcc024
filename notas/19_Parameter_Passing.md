# Parameter Passing

------
What are formal and actual parameters?

------
How does the language match up parameters?
- position
- key words: `ADA (DIVIDEND => X, DIVISOR => Y)`

How to implement it in Python?
```py
def div(dividend=1.0, divisor=1.0):
  r = dividend / divisor
  print "Result = ", r
```

What is the value of each call below?
```py
>>> div()
Result =  1.0
>>> div(dividend=3.2, divisor=2.3)
Result =  1.39130434783
>>> div(3.0, 1.5)
Result =  2.0
>>> div(3.0)
Result =  3.0
>>> div(1.5, divisor=3.0)
Result =  0.5
>>> div(1.5, dividend=3.0)
???
>>> div(dividend=3.0, 1.5)
???
```

* We can also have optional parameters:

------
What is this program going to print? **Mult.cpp**:
```cpp
#include <iostream>

class Mult {
  public:
    int f(int a = 1, int b = 2, int c = 3) const {
      return a * b * c;
    }
};

int main(int argc, char** argv) {
  Mult m;
  std::cout << m.f(4, 5, 6) << std::endl;
  std::cout << m.f(4, 5) << std::endl;
  std::cout << m.f(5) << std::endl;
  std::cout << m.f() << std::endl;
}
```

- or in C: **Variadic.c**

What is the program below doing?
```cpp
#include <stdio.h>
#include <stdarg.h>

int foo(size_t nargs, ...) {
  int a = 0, b = 0, c = 0, d = 0;
  int * arr[4];
  va_list ap;     // 3.2 What is ap?
  size_t i;
  arr[0] = &a;
  arr[1] = &b;
  arr[2] = &c;
  arr[3] = &d;
  va_start(ap, nargs);  // 3.3 What does va_start do?
  for(i = 0; i < nargs; i++) {
    * arr[i] = va_arg(ap, int); // 3.4 What does va_arg do?
  }
  va_end(ap);     // 3.5 And va_end?
                  // 3.6 What happens if I forget to call va_end?
  return a + b + c + d;
}
int main() {
  // 10,00000000000000000000000000000011
  unsigned long long int u = 8589934595;
  printf("%d\n", foo(0));
  printf("%d\n", foo(1, 5));
  printf("%d\n", foo(2, 5, 7));
  printf("%d\n", foo(3, 5, u, 1));
}
```

The default argument is evaluated only once. So, what is the meaning of
the function below?
```py
def f(a, L=[]):
  L.append(a)
  return L
```

How to ensure that we start always with a new list in the function above?
```py
def f(a, L=None):
  if L is None:
    L = []
  L.append(a)
  return L
```

------
Some languages allow a variable number of parameters in the function declaration. Examples?
What is the problem with that? **VP.c**
```c
#include "stdio.h"
int main(int argc, char ** argv) {
  printf("%s\n", argc);
}
```

* Strict vs Lazy languages:
```py
>>> False and 1 / 0 == 0
False

>>> def myand(a, b): return a and b
>>>  myand(False, 1/0 == 0)
ZeroDivisionError: integer division or modulo by zero
```

* Ways to pass parameters: (By)  
  - Strict:
    - value
    - result
    - value-result
    - reference
  - Lazy:
    - macro expansion
    - name
    - need

* Value:
------
What is this?
- The photocopy analogy.

What is the problem with the program below?
```c
#include "stdio.h"
void swap(int x, int y) {
  int aux = x;
  x = y;
  y = aux;;
}
int main() {
  int a = 2;
  int b = 3;
  printf("%d, %d\n", a, b);
  swap(a, b);
  printf("%d, %d\n", a, b);
}
```

------
How to implement a program that swaps two integers in C?
```c
#include "stdio.h"
void swap(int * x, int * y) {
  int aux = * x;
  * x = * y;
  * y = aux;
}
int main() {
  int a = 2;
  int b = 3;
  printf("%d, %d\n", a, b);
  swap(&a, &b);
  printf("%d, %d\n", a, b);
}
```

------
How to swap two variables without using an auxiliary location?
```c
void swap(int * x, int * y) {
  * x = * x ^ * y;
  * y = * x ^ * y;
  * x = * x ^ * y;
}
```

- Result: the parameter is uninitialized, and it is filled with a value before the function returns.

- Value result: same as result, but parameter is initialized.

- Reference:

------
What is this?  
What really is a variable?  
Which language has parameter passing by reference? **swap.cpp**:
```c

#include <stdio.h>
void swap(int &x, int &y) {
  x = x ^ y;
  y = x ^ y;
  x = x ^ y;
}
int main() {
  int a = 2;
  int b = 3;
  printf("%d, %d\n", a, b);
  swap(a, b);
  printf("%d, %d\n", a, b);
}
```

------
What is the advantage of being able to pass by reference?  
What is a problem of passing parameters by reference?
```c
int main() {
  int a = 2;
  int b = 3;
  printf("%d, %d\n", a, b);
  swap(a, a);
  printf("%d, %d\n", a, b);
}
```

What does this program do? **sigsum.cpp**:
```c
#include <stdio.h>
void sigsum(int& n, int& ans) {
  ans = 0;
  int i = 1;
  while (i <= n)
    ans += i++;
}
int main() {
  int x = 10;
  int y;
  sigsum(x, y);
  printf("x = %d, y = %d\n", x, y);
  x = 10;
  sigsum(x, x);
  printf("x = %d, y = %d\n", x, x);
  return 0;
}
```

How to prove that Java uses "by value"?

```java
public class Ref {
  public static void main(String args[]) {
    Object x = null;
    giveMeAString (x);
    System.out.println (x);
  }
  public static void giveMeAString (Object y) {
    y = "This is a string";
  }
}
```

How to prove that C++ uses "by value"?

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

static void giveMeAStringByRef (char*& s) {
  s = (char*)malloc(4 * sizeof(char));
  strcpy(s, "Oi!\n");
}
static void giveMeAStringByValue (char* s) {
  s = (char*)malloc(4 * sizeof(char));
  strcpy(s, "Oi!\n");
}
int main() {
  char* s1 = NULL;
  char* s2 = NULL;
  giveMeAStringByRef(s1);
  printf("%s", s1);
  giveMeAStringByValue(s2);
  printf("%s", s2);
}
```

- Macro expansion: the parameter is evaluated every time it is used.
```c
#include <stdio.h>
#define MAX(X, Y) ((X) > (Y) ? (X) : (Y))
int main() {
  int a = 2, b = 3;
  int c = MAX(a, b);
  printf("a = %d, b = %d, c = %d\n", a, b, c);
}
```

------
How to prove that the parameter is evaluated every time it is used?  
What is the advantage of macro expansion?  
What is the problem with macro expansion?

------
How to implement a swap with macro expansion?
```c
#include "stdio.h"
#define SWAP(X,Y) {int temp=X; X=Y; Y=temp;}
int main() {
  int a = 2;
  int b = 3;
  printf("%d, %d\n", a, b);
  SWAP(a, b);
  printf("%d, %d\n", a, b);
}
```

------
What is another problem with this?
```c
#include "stdio.h"
#define SWAP(X,Y) {int temp=X; X=Y; Y=temp;}
int main() {
  int a = 2;
  int temp = 17;
  printf("%d, temp = %d\n", a, temp);
  SWAP(a, temp);
  printf("%d, temp = %d\n", a, temp);
}
```

* The problem is variable capture:

------
What does this function do?

$(\lambda x.\lambda y.\lambda \text{ temp }.{x = y; y = \text{ temp }} x)$
- but -
$(\lambda x.\lambda y.\lambda \text{ temp }.{x = y; y = \text{ temp }} x) a \text{ temp }$

- Call by name: the parameter is always evaluated before each use, but in
the caller's context.
------
What is the result of the evaluation of:
```c
void f(by-name int a, by-name int b) {
  b=5;
  b=a;
}
int g() {
  int i = 3;
  f(i+1,i);
  return i;
}
```

How to implement call by name?

What will be printed by each parameter passing strategy: call by value, reference, value-result and name?
```py
def test (int x) {
  a[1] = 6;
  e = 2;
  x += 3;
}
main() {
  a[1] = 1; a[2] = 2; e = 1;
  test(a[e]);
  print a[1], " ", a[2], " ", e);
}
```

```py
$value:> 6 2 2
$reference:> 9 2 2
$value-result:> 4 2 2
$name:> 6 5 2
```

- Call by need: the parameter is evaluated once, and the result of the evaluation is cached:

------
What is the result of `f`?
```py
and (a, b) = if !a then false else b

g x = if g x then true else false

f = and (false, g)
```

Properties:
* The expression is only evaluated if the result is required by the calling function, called delayed evaluation.
* The expression is only evaluated to the extent that is required by the calling function, called Short-circuit evaluation.
* The expression is never evaluated more than once, called applicative-order evaluation.

* Example:
------
What is `fib 1 1`?
```py
fib m n = m : (fib n (m+n))

getIt [] _ = 0
getIt (x:xs) 1 = x
getIt (x:xs) n = getIt xs (n-1)
```

What is getIt (fib 0 1) 4?  
* Evaluating:

```py
getIt (fib 0 1) 4
getIt (0 : fib 1 1) 4

getIt (fib 1 1) 3
getIt (1 : fib 1 2) 3

getIt (fib 1 2) 2
getIt (1 : fib 2 3) 2

getIt (fib 2 3) 1
getIt (2 : fib 3 5) 1 = 2
```

------
How to distinguish between call by name and call by need?

------
Do you remember the concept of side-effect?
* Without side-effect, the parameter passing technique is immaterial.

-------
Theorem (Plotkin)
In the absence of side effects, lazy evaluation is the same as eager
evaluation given that the evaluation of the parameters terminate.
