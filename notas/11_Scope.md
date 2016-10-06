# Scope (Aula 11)

### How would be the world if everybody had a unique and different name?
Pros/Cons?

### What is the definition of a variable?
The definition of a variable is anything that establishes an association
between a name and a meaning.

Some examples:
- C++:
```cpp
const int Low = 1;
```

- C:
```c
typedef struct { int x, y;} Point;
```
- Pascal:
```pascal
var X: Ints;
```

### Is this function valid?
``fun s s = s * s;``

### What happens when we have many definitions for the same name?

* The scope of a definition is the region of the program where that
  definition is valid.

### How do we delimit the scope of variables?
- Blocks, e.g:
#### How do we create blocks in ML?
```
let val x = 1; val y = 2; in x + y end
fun cube x = x * x * x
         ^^^^^^^^^^^^^
```
* A block is a program region containing definitions of variables and
  that delimits the regions where these definitions apply.

#### How do we create blocks in C?

### What is the value of n below?

```
let
  val n = 1
  val x = n + 1
in
  let
    val n = 2
  in
    x + n
  end
end
```
* The scope of a definition is the block containg that definition, from
  the point of definition to the end of the block, minus the scopes of
  any redefinitions of the same name in interior blocks.

* Example:
```
let               A
  val n = 1       A
  val x = n + 1   A
in                A
  let             B
    val n = 2     B
  in              B
    n + x         B
  end             B
end               A
```
- or -
```c
#include <stdio.h>
int main() {
  int n = 1;
  {
    int n = 2;
    printf("%d\n", n);
  }
}
```
* **Namespaces**: named program regions used to limit the scope of variables
inside the program.
- Example: structures in ML:
```
structure Fred = struct val a = 1; fun f x = x + a; end;
Fred.a;
Fred.f 2;
```

### What are namespaces in Java (package, class) and C++
(namescape, class, struct)?

1) What is the difference between class and struct in C++?
   - There is no difference between a class and a struct except that by
  default, the members of a struct are public, and the members of a class
  are private. Also structs are by default inherited publicly and classes
  by default are inherited privately. Other than that, whatever you can
  do in a class, you can do in a struct.
2) What is the advantage of namespaces?
   - Information hiding: abstract data type.

* There are two ways to specify visibility:
  - Constructs to define visibility info in the namespace.
  e.g: Java: public, protected, private.
  - Separate namespace and interface,
  e.g: PASCAL: UNITS = Interface + Implementation.

* Orthogonal namespaces.

### What would happen if I had this construction in ML?

`val int = 3;`

1) What about:
```
fun square square = square * square;
```

- In ML we have a namespace for values, and another for types.
- In Java, packages, types, methods, fields and statement labels are in separate namespaces. Example:
  a) What is the meaning of each occurrence of the name Reuse in the
program below?
```java
class Reuse {
  Reuse Reuse (Reuse Reuse) {
    Reuse:
    for (;;) {
      if (Reuse.Reuse(Reuse) == Reuse)
        break Reuse;
    }
    return Reuse;
  }
}
```

b) Can I compile this program?
```c
int main() {
  int int = 0;
  printf("%d\n", int);
}
```

* There exists two basic type of scoping rules: static and dynamic.
  - Static scope associates each name to a definition at compilation time.
  - Dynamic scope does it at run time.

### What is the value of foo 5 below?
```
val one = 1;

fun inc x = x + one;

fun foo x =
  let
    val one = 2;
  in
    inc x
  end;

foo 5;
```

foo 5 will be 6 in ML, which uses static scoping, but it would be 7 if
ML was using dynamic scoping.

Few languages use dynamic scoping: APL and some dialects of LISP. This
kind of scoping has many disadvantages:
- It is difficult to implement efficiently.
- It makes it hard to understand the program.

For an example of APL code, see: http://catpad.net/michael/apl/

* Sharing definitions among different compilation units.

### How do I declare a variable in a C program that will be used in many different files?
```c
exVar.h:
int x = 3;

exVar.c:
#include "exVar.h"
extern int x;
int main() {
  x++;
  printf("%d\n", x);
}
```

In old Fortran, we had COMMON blocks, which must be declared the same way
in all the files:
```
File1.f
COMMON A, B, C
File2.f
COMMON X, Y, Z
Then, X = A, Y = B and Z = C
```

a) How do I share definitions in Java?
- Import statement:
```java
import java.math.*;
public class Test {
  public static void main(String[] a) {
    BigInteger f1 = new BigInteger("1");
    System.out.println(f1);
  }
}
```

b) How do I share definitions in C++?
Example 1:

```cpp
#include <iostream>
using namespace std;
namespace first {
  int var = 5;
}
namespace second {
  double var = 3.1416;
}
int main () {
  cout << first::var << endl;
  cout << second::var << endl;
  return 0;
}
```

Example 2:

```cpp
#include <iostream>
using namespace std;
namespace first {
  int x = 5;
  int y = 10;
}
namespace second {
  double x = 3.1416;
  double y = 2.7183;
}
int main () {
  using first::x;
  using second::y;
  cout << x << endl;
  cout << y << endl;
  cout << first::y << endl;
  cout << second::x << endl;
  return 0;
}
```

#### What are language libraries?
- A collection of definitions and implementations that provide ready-to-use
functions.

1) How are libraries implemented in C?
2) What about Java?

#### Libraries are a way to grow a programming language. Can you give
examples?

#### Implement a signature for a toggle switch, that is, an object that can hold two states {TOGGLED, UNTOGGLED}.

```
signature TOGGLER =
sig
  type toggler
  val new : toggler
  val toggle : toggler -> toggler
  val isToggled : toggler -> bool
  val numToggled : toggler -> int
end ;
```
#### Implement the toggler using a boolean type:

```
structure BoolTog :> TOGGLER =
struct
  type toggler = bool
  val new = false
  fun toggle t = if t then false else true
  fun isToggled t = if t then true else false
  fun numToggled t = 0
end ;
```
```
$> - val t = BoolTog.new;
$> val t = - : BoolTog.toggler
$> - val t = BoolTog.toggle t;
$> val t = - : BoolTog.toggler
$> - BoolTog.isToggled t;
$> val it = true : bool
```

#### The toggle object that we have just implemented has no memory, that is, one cannot remember how many times its state has been switched. Add this property to the signature and the implementation.

```
structure IntTog :> TOGGLER =
struct
  type toggler = int
  val new = 0
  fun toggle t = t + 1
  fun isToggled t = if t mod 2 = 0 then false else true
  fun numToggled t = t
end ;
```
Example:
```
$> - structure S = IntTog;
$> structure S : MTOG
$> - val t = S.new;
$> val t = - : IntTog.toggler
$> - val t = S.toggle t;
$> val t = - : IntTog.toggler
$> - val t = S.toggle t;
$> val t = - : IntTog.toggler
$> - S.numToggled t;
$> val it = 2 : int
```

#### Implement a function togN, that receives a TOGGLER, and returns the variable that results from toggling it N times:

```
fun togN 0 t = t
  | togN n t = togN (n - 1) (IntTog.toggle t);
```

#### What would happen?
```
- val b = BoolTog.new;
- val b = IntTog.toggle b;
```

* So far we have all the sml code in the same file. It is possible to separate
it into many files, using cm, the SML builder, to compile them together.

#### Build a CM file to compile all the files together:

```
Library
  structure BoolTog
  structure IntTog
is
  ToggleSig.sml
  BoolStr.sml
  IntStr.sml
```

#### How to load the contents of the cm file?
`- CM.make "sources.cm";`

* There exists different levels of strong typing:
1) C and C++ are not memory safe: I can cast a Battleship into a Pencil.
2) Python, JavaScript and Lua are memory safe, but they are not abstraction
safe: there is no way to encapsulate data.
3) SML, Java and C# are memory and abstraction safe.

#### Create a signature to represent sets of integers in SML:
```
signature SET =
sig
  type set
  val new : set
  val add : set -> int -> set
  val contains : set -> int -> bool
  val remove : set -> int -> set
end ;
```
#### In how many different ways can we implement this signature?

#### Create an implementation based on lists:

```
structure ListSet :> SET =
struct
  type set = int list
  val new = []
  fun add [] i = [i]
    | add (h::t) i =
        if h > i then i::h::t else if h < i then h :: add t i else (h::t)
  fun contains [] _ = false
    | contains (h::t) i = i = h orelse if h > i then false else contains t i
  fun remove [] _ = []
    | remove (h::t) i =
        if i = h then t else if h > i then h::t else h :: remove t i
end ;
```

#### Create an implementation based on boolean functions. Use closures:
```
structure FunSet :> SET =
struct
  type set = int -> bool
  val new = fn x => false
  fun add f i = fn x => if x = i then true else f x
  fun contains f i = f i
  fun remove f i = fn x => if x = i then false else f x
end ;
```

#### Create a cm file "sources.cm" to compile these implementations:
```
Library
  structure ListSet
  structure FunSet
is
  SetSig.sml
  ListSet.sml
  FunSet.sml
```

#### How to use this signature?
```
- CM.make "sources.cm";
- structure S = FunSet;
- val t = S.new;
- val t = S.add t 3;
- S.contains t 1;
- S.contains t 3;
- val t = S.remove t 3;
- S.contains t 3;
```
