# Object Oriented Programming

-------
Is the program below object oriented or not?
```py
class Node:
  def __init__(self):
    self.data = ""
    self.link = 0

class Stack:
  def __init__(self):
    self.top = 0

def add(stack, data):
  n = Node()
  n.data = data
  n.link = stack.top
  stack.top = n

def hasMore(stack):
  return stack.top != 0

def remove(stack):
  n = stack.top
  stack.top = n.link
  return n.data

s = Stack()
add(s, "AA")
add(s, "BB")
add(s, "CC")
while hasMore(s):
  print remove(s)
```

-------
How to recode it, to make it more object oriented?
```py
class Node:
  def __init__(self, data, link):
    self.data = data
    self.link = link

class Stack:
  def __init__(self):
    self.top = 0
  def add(self, element):
    n = Node(element, self.top)
    self.top = n
  def hasMore(self):
    return self.top != 0
  def remove(self):
    aux = self.top
    self.top = aux.link
    return aux.data

s = Stack()
s.add("AA")
s.add(2)
s.add("BB")
s.add([1, 2, 3])
while s.hasMore():
  print str(s.remove())
```

Couldn't we simply add the methods to the class table?

Considering the original implementation:
```py
Stack.add = add
Stack.hasMore = hasMore
Stack.remove = remove
s = Stack()
s.add("AA")
s.add("BB")
s.add("CC")
while s.hasMore():
  print s.remove()
```

-------
So, what is, in essence, object oriented programming?

-------
How did it came through, historically?

-------
What are the main characteristics of object oriented languages?
- Objects
- Messages (methods)
- Subtyping
  - Liskov's substitution principle
- Heavy heap usage

Some languages have these features:
- Classes
  - modules
  - types
- Inheritance
  - the open-closed principle
  - single inheritance
  - multiple inheritance
- Overloading
- Information hiding
- Run-time type interrogation
- Garbage Collection

-------
What is a class?
* A class is the implementation of an Abstract Data Type.
* A class is only text.
  - defines a compilation unit,
  - is a module,
  - is also a type.

-------
What is an object?
* An object is data.
* An object has a location in memory, has a state, needs to be created,
  might be destroyed.
* E.g., in Java, an object is an instance of a class.

-------
How does an object talks to another?
* Objects exchange messages.
* These messages are called 'methods'

-------
What is inheritance?
* Reuse mechanism.

-------
Create an Animal class hierarchy, in which a Dog is a Mammal, which is
an Animal.

```py
class Animal:
  "The base class of the whole object zoo."
  def __init__(self, name):
    self.name = name
  def __str__(self):
    return self.name + " is an animal"
  def eat(self):
    print self.name + ", which is an animal, is eating."

class Mammal(Animal):
  "Mammals are animals that know how to suck milk."
  def __str__(self):
    return self.name + " is a mammal"
  def suckMilk(self):
    print self.name + ", which is a mammal, is sucking milk."

class Dog(Mammal):
  "Dogs are mammals that can bark, and bite when they eat."
  def __str__(self):
    return self.name + " is a dog"
  def bark(self):
    print self.name + " is barking rather loudly."
  def eat(self):
    print self.name + " barks when it eats."
    self.bark

def test0():
  a = Animal("Tigrinho")
  m = Mammal("Oncinha")
  d = Dog("Mameluco")
  print a
  print m
  print d
  a.eat()
  m.suckMilk()
  d.bark()
```

-------
Can you think about a situation in which we would like to have multiple
inheritance?

```py
class Flier:
  "Fliers are entities that can fly."
  def __init__(self, name, height):
    self.name = name
    self.height = height
  def fly(self):
    print self.name + " is flying ", self.height, " metters high!"

class Bat(Flier, Mammal):
  "Bats are mammals that fly."
  def __str__(self):
    return self.name + " is a bat"
```

-------
When I call `__init__` over Bat, which method gets called? That from Flier, or that from Animal?

-------
What if the declaration of Flier were:
```py
class Bat(Mammal, Flier):
  "Bats are mammals that fly."
  def __str__(self):
    return self.name + " is a bat"
```

-------
Multiple inheritance has a couple problems. Have you ever heard of the
diamond inheritance problem?

* The diamond problem is an ambiguity that arises when two classes B and C
  inherit from A, and class D inherits from both B and C. If a method in D
  calls a method defined in A (and does not override the method), and B and
  C have overridden that method differently, then from which class does it
  inherit: B, or C?

Ex.:
```c
#include <iostream>
struct A { virtual void f() { std::cout << "I'm A!\n"; } };

struct B { virtual void f() { std::cout << "I'm B!\n"; } };

struct C : public A, public B {};

int main() {
  C* pc = new C;
  // pc->f();
  pc->A::f();
  pc->B::f();
}
```

-------
What is overloading?
```py
class Animal:
  ...
  def eat(self):
    print self.name + ", which is an animal, is eating."

class Dog(Mammal):
  ...
  def eat(self):
    print self.name + " barks when it eats."
    self.bark
```

Does python have true overloading?
- Not really: we can overwrite a method's name, but there will be only one
  implementation of that method reachable in the same base class.

-------
OO languages rely on dynamic binding. What is this?
* the target of a call is chosen at run-time.

-------
Which methods are called below?
```py
def test2():
  a1 = Animal("Tigrinho")
  a2 = Mammal("Oncinha")
  a3 = Dog("Mameluco")
  print a1
  print a2             
  print a3             
  a1.eat()             
  a2.suckMilk()        
  a2.eat()             
  a3.bark()
  a3.suckMilk()        
  a3.eat()
  a1.bark()
  a1 = a3
  a1.bark()
```

-------
What is subtyping polymorphism?

-------
What is the Liskov's substitution principle?
* if $S$ is a subtype of $T$, then $S$ can be used in any situation where $T$ is expected.

-------
Assuming the same class hierarchy, which methods are called in the Java program below? Answers include "does not compile", or "runtime error".
```java

  public void test () {
    Animal a1 = new Animal();
    Animal a2 = new Mammal();
    Animal a3 = new Dog();
    System.out.println(a1);
    System.out.println(a2);
    System.out.println(a3);
    a1.eat();
    a2.suckMilk();
    a2.eat();
    Dog d1 = a3;
    Dog d1 = new Dog();
    Mammal m1 = d1;
    d1.bark();
    m1.suckMilk();
    d1.suckMilk();
    Dog d2 = (Dog)a3;
    a3.bark();
    d2.bark();
    Dog d3 = (Dog)a2;
  }
```

-------
What is the difference between an object in Java and in Python?

-------
What is the complexity to find the target of a call in Python?

-------
What about the complexity to find the target of a call in Java?

-------
What is a virtual table?

-------
How do I know the type of a data-structure in C?

-------
How to know it in Python?
```py
  a = Mammal("Fortunato")
  if a.__class__.__name__ == Mammal.__name__:
    print "The thing is an animal!"
```

What is the difference between
```py
  a.__class__.__name__ == Animal.__name__ and isinstance(a, Animal)
```

-------
Why object oriented languages use so much the heap?

-------
Which mechanisms these languages use to facilitate the management of the heap?

-------
What is the essential difference between software development in function oriented languages and object-oriented languages?

**top down X bottom up**

-------
How is the design of a compiler according to these two paradigms?

Top down:
- translate source to machine code
...
1 Parse file
2 Optimize program
3 Generate machine code.
...
1.1 Open file
1.2 Read tokens
1.3 Produce AST
1.4 close files

-------
Where does data come in in the top-down approach?

Bottom up:
We need: "ParseTree", "Optimizer", "CodeGenerator", "Parser", "Lexer",
   "CodeGenerator", "ArchDescription", "File", "Token".

-------
What are the advantages of top-down?
- Structured and sistematic
- Global view
- Easier to explain

-------
What are the advantages of bottom up?
- Reusability
- Data independence
- No temporal ordering
- Easier to change

* In practice, programming is a mix of these two styles, but OO languages
  favour bottom-up because we can work with interfaces, instead of needing
  implementations.

-------
How should be a productive development environment?
- Refactoring
- Fast update
- Persistence
- Browsing
- Documentation
