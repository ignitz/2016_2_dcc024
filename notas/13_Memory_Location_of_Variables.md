# Memory Location of Variables (Aula 13)

1) What are the differences between the programs below?
```c
int f() {
  int a = 0;
  return a;
}
```
```
fun f() = let val a = 0 in a end;
```

2) Where do computers keep the values that programs manipulate?
- registers, cache L0, cache L1, Memory, Hard Disk

Example:
```c
#include <stdio.h>
int f(int a, int b, int c) {
  int aa = a+3;
  int bb = b << 2;
  int cc = c ^ 1001;
  return aa + bb + cc;
}
int main() {
  printf("%d\n", f(23, 41, 73));
}
```

```
$> gcc -s ex.c
```

2.1) What are these symbols eax, ebp, esp?

2.2) What these instructions are doing?
```as
movl    $73, 8(%esp)
movl    $41, 4(%esp)
movl    $23, (%esp)
call    f
```

Computers keep variables in memory.

3) Has anyone heard of the Von Neumman architecture?

3.1) What is it about?

3.2) Who was John Von Neumman?
- Define the Von Neumman architecture.

4) Has anyone heard of activation record?
- Activation: the lifetime of one execution of a function, from call to
  return.
- Activation Record: the information the program associates with a
  specific call to a function.

5) Which information is kept in activation records?
 - return address,
 - parameters,
 - return value.
 - link to caller's activation record.  

Define variable life time: it is the time during which a variable is alive.

6) Can the lifetime of a variable be greater than the lifetime of the function
where the variable is declared?
```c
int nextcount() {
  static int count = 0;
  count = count + 1;
  return count;
}
```

7) Where is located the activation record of a function?

7.a) How many activation records a function can have?

- Define static allocation: FORTRAN and COBOL.
  - show slide 15 of MPL
8) Has anyone heard of COBOL?
  - Explain Grace Hopper and COBOL.

Example of COBOL program:
```COBOL
ADD YEARS TO AGE.
MULTIPLY PRICE BY QUANTITY GIVING COST.
SUBTRACT DISCOUNT FROM COST GIVING FINAL-COST.
```

9) What is the advantage of static allocation?

9.a) What is the disadvantage?

10) How to support recursion?

10.1) How to support multi-threading?
- Define activation stack: a stack that traces the list of currently
  activated functions.
- Define stack frame: a set of memory addresses that are stacked when the
  functions is called.

- Show slides 20 -> 24
11) who invented merge-sort? John Von Neumann.
- show slides 25 -> 29

11.1) What is going to be printed by the program below?
```c
#include <stdio.h>

void function() {
  int v[0];
  v[3] = v[3] + 4;
}

int main() {
  int x;
  x = 13;
  function();
  x++;
  printf("%d\n",x);
}
```

11.2) Why does it behave like this?

12) What does it do?
```
fun foo n =
  let
    fun bar 0 = []
      | bar m = n :: bar (m - 1)
  in
    bar n
  end
```

12.a) Which 'n' is that in the body of 'bar'?

12.b) How to find it?

- Define nesting link: a link that points to the most recent activation of
the outer function.
- Draw activation records for foo 3
```
bar 3   /> foo 3:
m = 3  |   n = 3
raddr  |   ret addr
nlink_/    n link = NULL
prvAC----> prev.a.r = NULL
```

13) What is the algorithm to set the nesting link?
- Calling outer function: set to null
- Calling from outer to inner:
  set nesting link same as caller's activation record
- Calling from inner to inner:
  set nesting link same as caller's nesting link

14) How to find names if we have multi-levels of nesting?  
E.g: how does **f3** find the value of **n1** in the function below?

```
fun f1 n1 =
    let
      fun f2 n2 = let fun f3 n3 = n1 + n2 + n3 in f3 3 end
    in
      f2 5
    end
```

15) When you pass a function as a parameter, what really gets passed?
```
fun addXToAll (x,theList) =
  let
    fun addX y =
      y + x;
  in
    map addX theList
  end;
```

16.a) Which info is needed to compile this program?
- addXToAll calls map, map calls addX, and addX refers to a variable x in
  addXToAll's activation record.
- Function-values include both code to call, and nesting link to use
  when calling it.

Example: calling `addXToAll (4, [1,2,1,2])`

```
current activation record
|
v
parameter: 4, [1,2,1,2]  <---------------------------.
return address: 0x34A5CF32                           |
previous activation record = NULL                    |
nesting link = NULL                                  |
x = 4                                                |
theList = [1,2,1,2]                                  |
addX ==============================> y => y + x, nesting link
```

16) What happens when a value is used after the function where that value
is defined returns?
```c
#include <stdio.h>
unsigned* getMem() {
  unsigned x[10];
  int i;
  for (i = 0; i < 10; i++) {
    x[i] = i;
  }
  return x;
}
int main () {
  unsigned *p1 = getMem();
  unsigned *p2 = getMem();
  printf("[%u] = %d; [%u] = %d\n", p1, *p1, p2, *p2);
  printf("p2 = %u\n", p2[0]);
  p1[0] = 2;
  printf("p2 = %u\n", p2[0]);
}
```
<!-- ** -->

16.1) What would happen if I called another function at the end of the
program above?

But in SML this may happen, and it will be all right!
```
fun funToAddX x =
  let
    fun addX y = y + x
  in
    addX
  end

fun test n =
  let
    val f = funToAddX 3
  in
    f n
  end
```

17) funToAddX returns a function, but where is the 'x' used inside this
function, if the activation of funToAddX no longer exists when this
function is called?

- Show slide 48

18) Has anyone heard the word closure before?

19) How to implement **funToAddX** in *C*?
```c
#include <stdio.h>
#include <stdlib.h>
typedef struct struct_free_variables_addX {
  int x;
} FREE_VARIABLES_addX;
typedef struct {
  FREE_VARIABLES_addX* t;
  int (*f)(FREE_VARIABLES_addX* t, int x);
} CLOSURE_addX;
int addX(FREE_VARIABLES_addX* t, int y) {
  return y + t->x;
};
void* funToAddX(int x) {
  CLOSURE_addX* c = (CLOSURE_addX*) malloc (sizeof(CLOSURE_addX));
  c->t = (FREE_VARIABLES_addX*) malloc (sizeof(FREE_VARIABLES_addX));
  c->t->x = x;
  c->f = &addX;
  return c;
}
int test(int n) {
  CLOSURE_addX* c = funToAddX(3);
  int retVal = c->f(c->t, n);
  free(c->t);
  free(c);
  return retVal;
}
int main(int argc, char** argv) {
  printf("%d\n", test(argc));
  return 0;
}
```
