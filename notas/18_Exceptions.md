# Exceptions

Consider the method pop of a Stack. What can go wrong with this method?

-------
How to handle errors in programming languages?
- Use guards:
```java

public int pop() {
    Node n = top;
    top = n.getLink();
    return n.getData();
}
...
if (s.hasMore())
  s.pop();
```

- Total definition:
```java
  public int pop() {
    Node n = top;
    if (n==null) return 0;
    top = n.getLink();
    return n.getData();
  }
```

- Total definition is the approach adopted in the `atoi` function:
```c
  int main(int argc, char** argv) {
    int i = 1;
    for (; i < argc; i++) {
      int n = atoi(argv[i]);
      printf("%d\n", n);
    }
  }
```

```
./a.out abacate 12a4 345
```

- Fatal error:
```java
  public int pop() {
    Node n = top;
    if (n==null) {
      System.out.println("Popping an empty stack!");
      System.exit(-1);
    }
    top = n.getLink();
    return n.getData();
  }
```

- Error flagging
```c
#include <stdio.h>
#include <errno.h>
void checkArgs (int argc, char** argv) {
  if (argc < 2) {
    errno = 1;
  }
}
int main(int argc, char** argv) {
  checkArgs(argc, argv);
  if (errno == 1) {
    fprintf(stderr, "Syntaxe: %s file\n", argv[0]);
  } else {
    printf("Argument = %s\n", argv[1]);
  }
}
```

- Pre/pos conditions:
```
balance: INTEGER -- Current balance
deposit (sum: INTEGER)
  -- Add `sum' to account.
  require
  non_negative: sum >= 0
  do
  balance := balance + sum
  ensure
  --one_more_deposit: deposit_count = old deposit_count + 1
  updated: balance = old balance + sum
  end
```

- Exceptions:
```
exception BadArgument of int
fun fact n = if n = 0 then 1 else n * fact (n - 1)
fun fact1 n = if n < 0 then raise BadArgument n else fact n;
fun usefact1 n =
  "Answer = " ^ Int.toString (fact1 n)
  handle BadArgument n => "Bad argument " ^ Int.toString n;
```

-------
Code a program that reads two numbers, $n$ and $d$, and print $n/d$ in python:  
What are the concerns we must bear in mind when coding a robust application?

```py
class ArithmeticException(Exception):
  def __init__(self, msg):
    self.value = msg
  def __str__(self):
    return repr(self.value)

def div(n, d):
  if d == 0:
    raise ArithmeticException("Attempt to divide " + str(n) + " by zero")
  else:
    return n/d

while True:
  try:
    n = float(raw_input("Please, enter the dividend: "))
    d = float(raw_input("Please, enter the divisor: "))
    r = div(n, d)
    print "Result = ", r
  except ValueError:
    print "Invalid number format. Please, try again"
  except ArithmeticException, ae:
    print ae.value
    break
```

-------
What does this program do?

```java

public class TestException {
  public static void main(String[] args) {
    int i = Integer.parseInt(args[0]);
    int j = Integer.parseInt(args[1]);
    System.out.println(i / j);
  }
}
```

What about `java TestException 6 3`?  
What about `java TestException 6`?  
What about `java TestException 6 0`?  

-------
What is an exception?  
Conceptually?

-------
What are exceptions good for?

-------
In this previous program, who prints the error message?

-------
What is an exception in Java?
- object that inherits from `Throwable`
```
         Object
            |
            |
        Throwable
        /       \
       /         \
    Error     Exception
                  |
                  |
           RuntimeException
```

Error: serious errors, that one cannot recover from.
-------
Example?

Exception: errors that the programmer may want to catch, and that he can recover from. Some of them may be declared in methods, other may not. Example?

RuntimeException? General errors that can be thrown during program execution. They do not have to be declared in methods.

* Exceptions, at least as implemented in Java, are a kind of design pattern.

-------
What is a design pattern?  
What is the exception pattern made of?
- declaration : Exception BadArgument, extends Exception, etc
- throw       : throw, raise, etc
- try/catch   : catch, except, rescue, handle, etc

What does each of these mean?

-------
How do I capture an exception?  
How do I capture the ArithmeticException?
```java
public class TE1 {
  public static void main(String[] args) {
    try {
      int i = Integer.parseInt(args[0]);
      int j = Integer.parseInt(args[1]);
      System.out.println(i / j);
    } catch (ArithmeticException ae) {
      System.out.println("You are dividing by zero!");
      ae.printStackTrace();
    }
  }
}
```

* Exception handling has to do with the call stack: The runtime system searches the call stack for a method that contains a block of code that can handle the exception. This block of code is called an exception handler. The search begins with the method in which the error occurred and proceeds through the call stack in the reverse order in which the methods were called. When an appropriate handler is found, the runtime system passes the exception to the handler.

What about the `ArrayIndexOutOfBoundsException`?
```java
public class TE2 {
  public static void main(String[] args) {
    try {
      int i = Integer.parseInt(args[0]);
      int j = Integer.parseInt(args[1]);
      System.out.println(i / j);
    } catch (ArrayIndexOutOfBoundsException ie) {
      System.out.println("You are out of the array!");
      ie.printStackTrace();
    }
  }
}
```

What about all the exceptions?
```java
public class TE2 {
  public static void main(String[] args) {
    try {
      int i = Integer.parseInt(args[0]);
      int j = Integer.parseInt(args[1]);
      System.out.println(i / j);
    } catch (ArithmeticException ae) {
      System.out.println("You are dividing by zero!");
      ae.printStackTrace();
    } catch (ArrayIndexOutOfBoundsException ie) {
      System.out.println("You are out of the array!");
      ie.printStackTrace();
    }
  }
}
```

But These exceptions are subclasses of java.lang.Exception...
```java
public class TE3 {
  public static void main(String[] args) {
    try {
      int i = Integer.parseInt(args[0]);
      int j = Integer.parseInt(args[1]);
      System.out.println(i / j);
    } catch (Exception e) {
      System.out.println("You are dividing by zero!");
      e.printStackTrace();
    }
  }
}
```

-------
What happens after I execute the catch block?
```java
public class TE3 {
  public static void main(String[] args) {
    try {
      ...
    } catch (Exception e) {
      ...
    }
  }
  System.out.println("Done dividing the numbers!");
}
```

* Exceptions are handled by chains of **try/catches**

-------
Which catch block will be executed?
```java
public class TE2 {
  public static void main(String[] args) {
    try {
      int i = Integer.parseInt(args[0]);
      int j = Integer.parseInt(args[1]);
      System.out.println(i / j);
    } catch (ArithmeticException ae) {
      System.out.println("You are dividing by zero!");
      ae.printStackTrace();
    } catch (Exception e) {
      System.out.println("You are out of the array!");
      e.printStackTrace();
    }
  }
}
```

-------
And what would happen in this situation?
```java
public class TE4 {
  public static void main(String[] args) {
    try {
      int i = Integer.parseInt(args[0]);
      int j = Integer.parseInt(args[1]);
      System.out.println(i / j);
    } catch (Exception e) {
      System.out.println("You are out of the array!");
      e.printStackTrace();
    } catch (ArithmeticException ae) {
      System.out.println("You are dividing by zero!");
      ae.printStackTrace();
    }
  }
}
```

What is the algorithm that the compiler uses to verify that this program is wrong?

-------
What is an exception in Java?  
How do I declare an exception?
```java
public class MyException extends Error {}

public class MyTest {
  public static void m() {
    throw new MyException();
  }
  public static void main(String args[]) {
    m();
  }
}
```

How do I declare an exception in SML?
```
exception BadArgument of int
```

* It is better to declare the exception as subclass of Exception. Error is by default used to describe problems that the program should not (but could) recover from:

```java
public class MyException extends Exception {}

public class MyTest {
  public static void m() throws MyException {
    throw new MyException();
  }
  public static void main(String args[]) {
    try {
      m();
    } catch (MyException e) {
      e.printStackTrace();
    }
  }
}
```

Why did I have to specify that `m` throws `MyException`?

* I can add a constructor with a String to MyException:
```java
public class MyException extends Exception {
  public MyException() {
  }
  public MyException(String msg) {
    super(msg);
  }
}

public class MyTest {
  public static void m() throws MyException {
    throw new MyException("This is an error!");
  }
  public static void main(String args[]) {
    try {
      m();
    } catch (MyException e) {
      e.printStackTrace();
      System.out.println("\n--//--\n");
      System.out.println(e.getMessage());
    }
  }
}
```

-------
What is the semantics of this word `super()`?

-------
What happens if I remove the **try/catch** block from the program above?

* Some exceptions are checked, others are unchecked.
```
         Object
            |
        Throwable
        /        \
     *Error    Exception
                  |
          *RuntimeException
```

* A method that can get a checked exception cannot ignore it.
-------
What can this method do?
- try and catch it. - or -
- pass forward:
```java
public class MT {
  public static void m() throws MyException {
    throw new MyException("This is an error!");
  }
  public static void main(String args[]) throws MyException {
    m();
  }
}
```

-------
What are the advantages of checked exceptions?
- documentation

What about disadvantages.
- extra typing. Perhaps clogged code.

-------
What does this program do? What about `Error()`?
```java
public class MT1 {
  public static void main(String args[]) {
    System.out.println("1");
    try {
      System.out.println("2");
      if (true) throw new Exception();
      // if (true) throw new Error();
      System.out.println("3");
    } catch (Exception e) {
      System.out.println("4");
    } finally {
      System.out.println("5");
    }
    System.out.println("6");
  }
}
```

-------
And what about this program?
```java
public class MT2 {
  public static int f(boolean t) {
    try {
      if (t) throw new Exception();
      return 1;
    } catch (Exception e) {
      System.out.println("2");
    } finally {
      System.out.println("3");
    }
    System.out.println("4");
    return 2;
  }
  public static void main(String args[]) {
    System.out.println("1");
    f(false);
  }
}
```

-------
What about this?
```java
public class Finally {
  public static int foo(boolean p) {
    int r = 0;
    try {
      if (p) throw new Exception();
      r = 1;
      return r;
    } catch (Exception e) {
      r = 2;
    } finally {
      return 3;
    }
  }
  public static void main(String args[]) {
    System.out.println(foo(Boolean.valueOf(args[0])));
  }
}
```

And this program, what does it do?
```java
public class MT1 {
  static class R {
    int x;
  }
  public static R foo(boolean p) {
    R r = new R();
    r.x = 0;
    try {
      if (p) throw new Exception();
      r.x = 1;
      return r;
    } catch (Exception e) {
      r.x = 2;
    } finally {
      r.x = 3;
    }
  }
  public static void main(String args[]) {
    System.out.println(foo(Boolean.valueOf(args[0])).x);
  }
}
```

-------
Quick recap: how does a method propagate an exception to another method?

-------
What is the finally block good for?

-------
How to code safely in languages that do not provide `finally`? E.g: C++ "Resource Acquisition Is Initialization"
