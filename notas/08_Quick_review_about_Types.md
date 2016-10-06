# Quick review about Types (Aula 08)

### Types can be checked at two different moments:
- Dynamically: during the program execution. E.g: php, python, ruby, bash.
- Statically: during the compilation of the program. E.g: Java, C, C++, SML.

### There exists languages which are strongly typed, and languages which are weakly typed.
- A strongly typed language guarantees that a type will be always used
as declared.
- A weakly typed language allows a value to be used as if it had a type
  different than its declared type.

### There are two ways to decide when two types are equivalente:
- name equivalence: two types are the same, if, and only if, they have the
  same name: C, Java, C++, etc.
- structural equivalence: two types are the same if they have the same
  structure. Example: SML.

Ex. in C:
```c
typedef struct { int a; float b; } T0;
typedef struct { int a; float b; } T1;
int foo(T1 s) { printf("%d, %f\n", s.a, s.b); }
int main() {
  T0 x;
  T1 y;
  foo(y);
  foo(x); // Does not compile
}
```

Ex. in SML:
```
- type T0 = int * real;

  type T0 = int * real

- type T1 = int * real;

  type T1 = int * real

- fun foo (s:T1) = #1 s;

  val foo = fn : T1 -> int

- val x:T0 = (1, 3.14);

  val x = (1,3.14) : T0

- foo x;

  val it = 1 : int
```

C tem tipagem nominal
ML tem tipagem estrututal


### Compilers use three different strategies to statically discover the types of symbols:

- Annotations: the programmer must explicitly write the type of a symbol
  next to it. Examples: Java, C, C++.

- Inference: the compiler uses an algorithm that finds the correct type of
  each value. Examples: Haskell, Scala, SML.

- Special names: In some old languages, the name of the variable gives away
  its type. Example: in old Fortran, integer variables should start with 'I'.

### Some languages allow runtime type tests.

1) Can anyone give me an example?

```php
<?php
$c = 1;
if ($c == 1) {
  $var = true;
} else if ($c == 2) {
  $var = 42;
} else if ($c == 3) {
  $var = 3.14;
} else {
  $var = "dcc024";
}
echo "Is int? " . is_int($var) . "<br>";
echo "Is bool? " . is_bool($var) . "<br>";
echo "Is float? " . is_float($var) . "<br>";
echo "Is string? " . is_string($var) . "<br>";
?>
```

Polymorphism
============

1) What is the type of this C function?

```c
int f (int selector, char a, char b) {
  return selector ? a : b;
}
```

1.1) What about this ML function?

```ml
fun f (selector, a, b) = if selector then a else b;
```

### A function or operator is polymorphic if it has at least two possible types

- It exhibits ad hoc polymorphism if it has at least two but only finitely many possible types
  - Overloading
  - Coercion
- It exhibits universal polymorphism if it has infinitely many possible types.
  - Subtyping
  - Parametric

2) What is overloading?

2.1) Can anyone give me an example?

```c
// over.cpp
#include <iostream>
int sum(int a, int b) {
  std::cout << "Sum of ints\n";
  return a + b;
}
double sum(double a, double b) {
  std::cout << "Sum of doubles\n";
  return a + b;
}
int main() {
  std::cout << "The sum is " << sum(1, 2) << std::endl;
  std::cout << "The sum is " << sum(1.2, 2.1) << std::endl;
}
```

2.2) What if I change the doubles to ints?

3) How to implement overloading? E.g, what the program above would look like
in assembly?

```sh
$ g++ -S over.cpp
```

### Many languages have overloaded operators.

**There are languages that allow the programmer to change the meaning of operators:**

3.1) Consider the class **MyString** below:

```cpp
// opOver.cpp
class MyString {
  public:
  static const int CAP = 100;
  MyString (const char* arg) {
    strncpy(member1, arg, CAP);
  }
  private:
  char member1[CAP];
};
```

3.1) How to add a *'+'* operator to this class so that *s1 + s2* has the effect
of appending the contents of *s2* into *s1*, and storing the result into s1?

```c
void operator +(MyString val) {
  strcat(member1, val.member1);
}
```

3.2) How to add a *<<* operator to this class, so that it prints the object?

```c
friend std::ostream & operator<<(std::ostream & os, const MyString & a) {
  os << a.member1;
}
```

**Example of usage:**

```cpp
int main () {
  MyString s1("UF");
  MyString s2("MG");
  s1 + s2;
  std::cout << s1 << std::endl;
}
```

In **sml** we can overwrite a symbol or a name, but we cannot have two user
defined implementations with the same name:

```
- infix 3 +;
  infix 3 +
- fun op + (a, b) = a - b;
  val + = fn : int * int -> int
- 3 + 2;
  val it = 1 : int
```

4) Oops: how to recover the original meaning of the plus symbol?

```ml
fun op + (a, b) = a - (~b);
```

4) What is a type coercion?

4.1) Can anyone give me one example?

```cpp
double sumX(double a, double b) {
  return a + b;
}
int main() {
  std::cout << "The sum is " << sumX(1, 2.0) << std::endl;
}
```

5) Which calls are legal?

```java
public class Coercion {
  public static void f(double x) {
    System.out.println(x);
  }
  public static void main(String args[]) {
    f((byte)1);
    f((short)2);
    f('a');
    f(3);
    f(4L);
    f(5.6F);
    f(5.6);
  }
}
```

5.1) Why we get 5.599999904632568 when we try to print 5.6F?

### Tricky iterations:

- overloading uses the types to choose the definition.
- Coercion uses the definition to choose a type conversion.

6) Which functions get called?

```cpp
// square.cpp
#include <iostream>
int square(int a) {
  std::cout << "Square of ints\n";
  return a * a;
}
double square(double a) {
  std::cout << "Square of doubles\n";
  return a * a;
}
int main() {
  double b = 'a';
  int i = 'a';
  std::cout << square(b) << std::endl;
  std::cout << square(i) << std::endl;
  std::cout << square('a') << std::endl;
}
```

7) Which sum is called in the file over.cpp?

```cpp
std::cout << "The sum is " << sum(1, 2.0) << std::endl;
```

8) What is the type of:

```ml
List<String> l
fun f a b = a
```

9) Why does parametric polymorphism has this name?

9.1) Code a function **getMax** in **C++** that returns the maximum of two elements:

```cpp
template <class T>
T GetMax (T a, T b) {
  T result;
  result = (a > b)? a : b;
  return (result);
}
```

9.2) How to call this function?

```cpp
// template.cpp:
int main () {
  int i = 5, j = 6, k;
  long l = 10, m = 5, n;
  k = GetMax <int> (i,j); // see the type parameter!
  n = GetMax <long> (l,m);
  cout << k << endl;
  cout << n << endl;
  return 0;
}
```

9.3) How to implement the function **getMax** in **SML**? Is this implementation
polymorphic? Why?

9.4) The **GetMax** function could be implemented as a macro. How?

```c
#define GetMax(a,b) ((a) < (b) ? (b) : (a))
```

9.5) What are the advantages and disadvantages of using macros over templates?

9.6) How to create a **C++** class **MyInt**, with the operator *>*, so that we can
apply **getMax** on it?

```cpp
class MyInt {
  friend std::ostream & operator<<(std::ostream& os, const MyInt& m) {
    os << m.data;
  }
  friend bool operator >(MyInt& mi1, MyInt& mi2) {
    return mi1.data > mi2.data;
  }
  public:
    MyInt(int i) : data(i) {}
  private:
    const int data;
};
```

9.7) And how to use it?

```cpp
  MyInt m1(50), m2(56);
  MyInt mi = GetMax<MyInt>(m1, m2);
  cout << mi << endl;
```

What would happen if **MyInt** did not have the operator *'<'* defined?

10) What is a type constructor?

11) How is parametric polymorphism implemented in C++?
11.1) How is it implemented in Java?

12) What is the type of ` fun comp a b = a = b;`

```
stdIn:1.18 Warning: calling polyEqual
val comp = fn : ''a -> ''a -> bool
```

12) Which type of polymorphism is happening here?

```java
public class Sub {
  public static void print(Object o) {
    System.out.println(o);
  }
  public static void main(String[] a) {
    print(new String("dcc024"));
    print(new Integer(42));
    print(new Character('a'));
  }
}
```

13) Has anyone heard of **Ocaml**?
- It is a functional, object-oriented language, that came out of ML, and that
  uses structural typing.

13.1) How to create objects in **Ocaml**?

```ocaml
let x =
   object
     val mutable x = 5
     method get_x = x
     method set_x y = x <- y
   end;;

let y =
   object
     method get_x = 2
     method set_x y = Printf.printf "%d\n" y
   end;;
```

13.2) How to run a program?

```sh
$ ocaml
# #use "sub.ml";;;
```

13.3) What happens if I try: `# x = y;;`

13.4) Does this expression type-checks?

13.5) What does this definition do?

```ocaml
let set_to_10 a = a#set_x 10;;
```

13.6) Is it legal?
`# set_to_10 x;;`
- or -
`# set_to_10 y;;`

13.7) Consider, now, the **z** object below:

```ocaml
let z =
   object
     method blahblah = 2.5
     method set_x y = Printf.printf "%d\n" y
   end;;
```

Is it legal? `set_to_10 z;;`

13.8) Consider, now, a new type definition:

```ocaml
type simpler_obj = < get_x : int >;;
```

Is it legal to convert x to simpler_obj?

```ocaml
# let w = (x :> simpler_obj);;
# w#get_x;;
```

13.9) What about converting z to simpler_obj?
```ocaml
(z :> simpler_obj);;
```

### Summary:
- Overloading: each different type requires a separate definition.
- Coercion: all the conversions must be defined in the language spec.
- Parametric: infinite as long as the universe over which type variables are instantiated is infinite.
- Subtype: infinite, as long as there is no limit on the number of subtypes of a type.

12) Why do programming languages have polymorphism?
