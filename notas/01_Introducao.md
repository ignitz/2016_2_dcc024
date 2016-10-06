# Introdução (Aula 01)

```
Programming Languages
      /            \
Imperative       Declarative
                  /       \
            Functional   Logic
```

### Imperative languages:
 - state + instructions.
 - Recipes.
 - Emphasis on HOW to do something.
 - Follow the Turing Machine.
 - Program is a sequence of instructions that explain how to change the
   state.
 - Variable assignment is the main way to change the state.
 - Typical language constructs are: ``if, while, for, case, etc``.
 - The hardware is imperative.
 - Due to many reasons, this is the dominant programming paradigm.
 - Mainstream object oriented languages such as Java, Perl, C++ and C#
   are imperative.

### Declarative languages:
 - Emphasis on WHAT must be done.
 - Programs as proofs.
 - Referential transparency.
 - No side effects: makes it easier to reason about the program.
 - Functional Programming.
 - Logic Programming.

### Functional Programming
 - Lambda-calculus
 - Computation as the evaluation of mathematical functions.
 - No state.
 - no mutable data.
 - Referential transparency.
 - Higher order functions.

### Logic Programming
 - Program as axioms and derivation rules.
 - Program is interpreted by a theorem prover.
 - Procedural/Declarative view, e.g, given ``H := B1, ... Bn``
   - To prove H, you must first prove B1, ..., Bn
   - or if B1 is true and ... and Bn is true, then H is true.

#### Example 1: Concatenating two lists.
a) Java
```java
interface List<E> {
  public E getHead();
  public List<E> getTail();
  public boolean isEmpty();
  public String toString();
}

class NullList<E> implements List<E> {
  public E getHead() {
    throw new Error("The null list has no head");
  }
  public List<E> getTail() {
    throw new Error("The null list has no tail");
  }
  public boolean isEmpty() {
    return true;
  }
  public String toString() {
    return "";
  }
}

class NodeList<E> implements List<E> {
  private E head;
  private List<E> tail;
  public NodeList(E e, List<E> t) {
    head = e;
    tail = t;
  }
  public E getHead() {
    return head;
  }
  public List<E> getTail() {
    return tail;
  }
  public boolean isEmpty() {
    return false;
  }
  public String toString() {
    return head + " " + tail.toString();
  }
}

public class Append {
  public static <E> List<E> appendImp(List<E> l1, List<E> l2) {
    List<E> invl = new NullList<E>();
    List<E> lr = l2;
    // Invert the list l1 to produce the list invl:
    while (!l1.isEmpty()) {
      invl = new NodeList<E>(l1.getHead(), invl);
      l1 = l1.getTail();
    }
    // append the inverted list, from tail to head, into l2.
    while (!invl.isEmpty()) {
      lr = new NodeList<E>(invl.getHead(), lr);
      invl = invl.getTail();
    }
    return lr;
  }

  public static <E> List<E> appendFunc(List<E> l1, List<E> l2) {
    if (l1.isEmpty()) {
      return l2;
    } else {
      return new NodeList<E>(l1.getHead(), appendFunc(l1.getTail(), l2));
    }
  }

  public static void main(String args[]) {
    List<Integer> l1 = new NullList<Integer>();
    for (int i = 10; i > 0; i--) {
      l1 = new NodeList<Integer>(i, l1);
    }
    System.out.println(l1);
    List<Integer> l2 = new NullList<Integer>();
    for (int i = 20; i > 10; i--) {
      l2 = new NodeList<Integer>(i, l2);
    }
    System.out.println(l2);
    System.out.println(appendFunc(l1, l2));
  }
}
```

Declarative way:
```
append(l1:List, l2:List):List
append(nil, l2) => l2
append([a|Rest], l2) => a | append(Rest, l2)
```

b) ML
```
fun append [] l = l
  | append (a::r) l = a :: append r l
```

c) Prolog
```
append([], L, L).
append([A|R], L, [A|RL]) :- append(R, L, RL).
```

#### Example 2: side effects makes it difficult to reason about programs:
```c
int p;
int f() {
  return p++;
}
int main() {
  int i1, i2;
  p = 0;
  i1 = f();
  i2 = f();
  printf("%d, %d\n", i1, i2);
}
```

#### Example 3: Quicksort

a) Implementation in C:
```c
void swap(int *a, int *b) {
  int t=*a;
  *a=*b;
  *b=t;
}


void sort(int arr[], int beg, int end)  {
  int middle, l, r;
  if (end > beg + 1) {
    middle = arr[(beg+end)/2];
    l = beg;
    r = end;
    while (l < r) {
      while (arr[l]<middle) l++;
      while (arr[r]>middle) r--;
      if (l < r) {
        swap(&arr[l], &arr[r]);
        l++;
        r--;
      }
    }
    sort(arr, beg, r);
    sort(arr, l, end);
  }
}
```

b) Implementation in ML:
```
fun partition (pivot, nil) = (nil, nil)
  | partition (pivot, first::others) =
    let
      val(smalls, bigs) = partition(pivot, others)
    in
      if first < pivot
      then (first::smalls, bigs)
      else (smalls, first::bigs)
    end ;

fun qsort nil = nil
  | qsort [singleton] = [singleton]
  | qsort (first::rest) =
    let
      val (smalls, bigs) = partition(first, rest)
    in
      qsort(smalls) @ [first] @ qsort(bigs)
    end ;
```

#### Example 4: The imperative versus the functional style in the language Scala:

a) Imperative quicksort:

```
def sort(xs: Array[Int]) {
  def swap(i: Int, j: Int) {
    val t = xs(i); xs(i) = xs(j); xs(j) = t
  }
  def sort1(l: Int, r: Int) {
    val pivot = xs((l + r) / 2)
    var i = l; var j = r
    while (i <= j) {
      while (xs(i) < pivot) i += 1
      while (xs(j) > pivot) j -= 1
      if (i <= j) {
        swap(i, j)
        i += 1
        j -= 1
      }
    }
    if (l < j) sort1(l, j)
    if (j < r) sort1(i, r)
  }
  sort1(0, xs.length - 1)
}
```

b) Functional quicksort: the use of higher-order functions make the program
much smaller.

```
def sort(xs: Array[Int]): Array[Int] =
  if (xs.length <= 1) xs
  else {
    val pivot = xs(xs.length / 2)
    Array.concat(
      sort(xs filter (pivot >)),
      xs filter (pivot ==),
      sort(xs filter (pivot <)))
  }
```
