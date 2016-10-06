# Pattern machining (Aula 06)

A pattern is a syntactical expression that can be matched against other
syntactical expressions.  
Ex.:
```
fun f n = n * n
fun f (a, b) = a * b;
```

* We can use wild-cards:

1) What is the problem with `fun first a b = a;`

* Constants can be patterns:  
Ex.:
```
fun f 0 = 2;
```

2) What happens if I try `f 4`?  
2.1) What is the problem with `fun f 0.0 = 2;` ?

- Tuples can be patterns:

3) What are the values of x and y in each case?
```
val (x, y) = (4, "asdf");
val (x, y) = ([2,3,4], ("dcc024", 3.14));
```

* We can use lists as patterns too:
```
fun f [a, _] = a;
```
4) What is the type of this function?

- Also, we can use cons as a pattern.
```
fun f (x::xs) = x;
```

5) How to implement the **hd** function?
```
- fun hd (h::t) = h;
```

- Using a wildcard:
```
- fun hd (h::_) = h;
```

5.1) How to implement a function that returns the second element of a list?

5) Which patterns have we seen so far?
- A variable is a pattern that matches anything, and binds to it.
- A _ is a pattern that matches anything.
- A constant (of an equality type) is a pattern that matches
  only that constant.
- A tuple of patterns is a pattern that matches any tuple of the
  right size, whose contents match the sub-patterns.
- A list of patterns is a pattern that matches any list of the
  right size, whose contents match the sub-patterns.
- A cons (::) of patterns is a pattern that matches any nonempty
  list whose head and tail match the sub-patterns.

* A function may have many patterns:
```
fun f 0 = "zero"
  | f 1 = "one"
```

6) How to code a function that prints zero if the only argument is 0, and
prints non-zero otherwise?
```
fun f 0 = "zero"
  | f _ = "non-zero"
```

6.1) How to write this function using if-then-else?
```
fun f n = if n = 0 then "zero" else "non-zero";
```

7) How to write the factorial function using pattern matching?

8) How to write the reverse function?
```
fun rev nil = nil
  | rev (h::t) = rev t @ [h]
```

9) A function to sum up the elements of a list

10) A function to count the number of true elements in a list of bools:
```
fun f nil = 0
  | f (true::rest) = 1 + f rest
  | f (false::rest) = f rest;
```

11) A function to increment all the elements of a list:

12) A function that takes a pair of elements, and returns true if they are
the same:  
12.1) What is the problem with `fun f(a, a) = true | f(a, _) = false` ?

* We can also use patterns in val declarations:
```
val (a, b) = (1, 2.3)
val a::b = [1,2,3,4,5]
```

* We can create local variables with the '**let**' block:
```
let val x = 1 val y = 2 in x + y end;
```

13) What is the value of **x** ?

14) How to write a function that converts days to miliseconds?
```
fun days2ms days =
  let
    val hours = days * 24.0
    val minutes = hours * 60.0
    val seconds = minuts* 60.0
  in
    seconds * 1000.0
  end;
```

15) How to code a function that splits a list in two pairs, e.g:
```
halve [1,2,3,4] = ([1,3], [2,4])
```

* We can load files in the interpreter, using the "use" command.
```
fun halve nil = (nil, nil)
  | halve [a] = ([a], nil)
  | halve (a::b::cs) =
    let
      val (x, y) = halve cs
    in
      (a::x, b::y)
    end;
```

15.1) Can you prove that the lists are ballanced?  
Hint: use induction.

16) Let's write a merge-sort? First, we need a way to merge two sorted lists of
integers:  
```
fun merge (nil, ys) = ys
  | merge (xs, nil) = xs
  | merge (x::xs, y::ys) =
    if (x < y) then x :: merge(xs, y::ys)
    else y :: merge (x::xs, ys);
```

16.1) What if the elements are not sorted?

17) How do I conclude, and build the merge-sort?
```
fun mergeSort nil = nil
  | mergeSort [a] = [a]  (* 17.1 What happens if I delete this line?  *)
  | mergeSort theList =
    let
      val (x, y) = halve theList
    in
      merge(mergeSort x, mergeSort y)
    end;
```

18) How do I write comments in ML? What about other languages?

19) Write a function to compute the Fibonacci numbers:
```
fun fib 0 = 0
  | fib 1 = 1
  | fib n = (fib (n-1)) + (fib (n-2)) ;
```

20) Can it be made more efficient?
```
fun fastFib n =
  let
    fun fibsFrom 0 = [0]
      | fibsFrom 1 = [1, 0]
      | fibsFrom n =
        let
          val a::b::l = fibsFrom (n-1)
        in
          ( a + b ) :: (a::b::l)
        end
  in
    hd (fibsFrom n)
  end
```

20.1) Can you do this fast fib with tuples, instead of lists?
```
fun fastFib n =
  let
    fun fibsFrom 0 = (0, 0)
      | fibsFrom 1 = (1, 0)
      | fibsFrom n =
        let
          val (a, b) = fibsFrom (n-1)
        in
          ( a + b , a)
        end
  in
    #1 (fibsFrom n)
  end
```

21) Quicksort?
```
fun partition (pivot, nil) = (nil,nil)
  | partition (pivot, first :: others) =
       let
         val (smalls, bigs) =
           partition(pivot, others)
       in
         if first < pivot
           then (first::smalls, bigs)
           else (smalls, first::bigs)
       end;

fun qsort nil = nil
  | qsort (first::rest) =
      let
        val (smalls, bigs) = partition(first,rest)
      in
        qsort(smalls) @ [first] @ qsort(bigs)
      end;
```
