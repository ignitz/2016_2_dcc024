# Language systems (Aula 04)

1) What is the life cycle of a program?
- Write the program using a text editor.
- Compile the program to machine code:
  - Compile the program to assembly.
  - Translate assembly to machine code.
  - Link with libraries.
  - Load program in memory, and replace names with addresses.
- Debug the program.

The classical Sequence:  
```
[editor] --> source file --> [preprocessor] --> preprocessed source file
--> [compiler] --> assembly language file --> [assembler] -->
object file --> [linker] --> executable file -->
[loader] --> running program in memory
```

1.1) What is assembly code?

1.2) What is machine code?

1.3) What is really the command gcc?

In the example below, let's consider the following C file:
```c
/* cube.c */
#include <stdio.h>
#define CUBE(x) (x)*(x)*(x)
int main() {
  int i = 0;
  int x = 2;
  int sum = 0;
  while (i++ < 100) {
    sum += CUBE(x);
  }
  printf("The sum is %d\n", sum);
}
```

1.3) How to produce the preprocessed code from a c file?
```
gcc -E cube.c > cube.p.c
```

1.3.1) Why does it have so many lines?

1.3.2) Which declarations will be in these lines?

1.3.3) What if I remove the #include from the program?

1.4) How to produce an assembly program from a c file?
```
gcc -S cube.p.c
- or in cpp -
 /usr/libexec/gcc/i686-pc-linux-gnu/4.1.2/cc1 cube.p.c -o cube.p.s
 - or in rhyme or doce.grad -
 /usr/lib/gcc/i486-linux-gnu/4.4.3/cc1 cube.p.c -o cube.p.s
```

1.4.1) Where is "The sum is %d"?

1.4.2) Where is the loop?

1.4.3) Is this program efficient?

1.5) How to produce an object file?
```
as cube.p.s -o cube.o
```

1.5.1) Where is "The sum is %d"?

1.5.2) Where is printf?

1.6) How to link with the external libraries?
```
/* In cpp: */
/usr/libexec/gcc/i686-pc-linux-gnu/4.1.2/collect2 --eh-frame-hdr -m elf_i386
-dynamic-linker /lib/ld-linux.so.2
/usr/lib/gcc/i686-pc-linux-gnu/4.1.2/../../../crt1.o
/usr/lib/gcc/i686-pc-linux-gnu/4.1.2/../../../crti.o
/usr/lib/gcc/i686-pc-linux-gnu/4.1.2/crtbegin.o
-L/usr/lib/gcc/i686-pc-linux-gnu/4.1.2
-L/usr/lib/gcc/i686-pc-linux-gnu/4.1.2
-L/usr/lib/gcc/i686-pc-linux-gnu/4.1.2/../../../../i686-pc-linux-gnu/lib
-L/usr/lib/gcc/i686-pc-linux-gnu/4.1.2/../../.. -lgcc --as-needed -lgcc_s
--no-as-needed -lc -lgcc --as-needed -lgcc_s --no-as-needed
/usr/lib/gcc/i686-pc-linux-gnu/4.1.2/crtend.o
/usr/lib/gcc/i686-pc-linux-gnu/4.1.2/../../../crtn.o cube.o -o cube.exe

/* In rhyme */
/usr/lib/gcc/i486-linux-gnu/4.4.3/collect2 --build-id --eh-frame-hdr
-m elf_i386 --hash-style=both -dynamic-linker /lib/ld-linux.so.2 -z relro
/usr/lib/gcc/i486-linux-gnu/4.4.3/../../../../lib/crt1.o
/usr/lib/gcc/i486-linux-gnu/4.4.3/../../../../lib/crti.o
/usr/lib/gcc/i486-linux-gnu/4.4.3/crtbegin.o
-L/usr/lib/gcc/i486-linux-gnu/4.4.3 -L/usr/lib/gcc/i486-linux-gnu/4.4.3
-L/usr/lib/gcc/i486-linux-gnu/4.4.3/../../../../lib
-L/lib/../lib -L/usr/lib/../lib -L/usr/lib/gcc/i486-linux-gnu/4.4.3/../../..
-L/usr/lib/i486-linux-gnu -lgcc --as-needed -lgcc_s --no-as-needed -lc -lgcc
--as-needed -lgcc_s --no-as-needed /usr/lib/gcc/i486-linux-gnu/4.4.3/crtend.o
/usr/lib/gcc/i486-linux-gnu/4.4.3/../../../../lib/crtn.o cube.o -o cube.exe
```

1.6.1) Why is the executable so much larger than the object?

1.6.2) what are the differences between the object and the executable?

2) Compilers do some optimizations. How to optimize the code below?
```c
int i = 0;
while (i < 100) {
  a[i++] = x*x*x;
}
```
```
/* In cpp */
/usr/libexec/gcc/i686-pc-linux-gnu/4.1.2/cc1 cube.p.c -o cube.p.s -O1

/* In rhyme */
/usr/lib/gcc/i486-linux-gnu/4.4.3/cc1 cube.p.c -o cube.p.s -O1
```

2.1) What is this new program doing?

2.2) Is there any disadvantage in doing some optimization?
- It makes it harder to see what assembly is produced for each statement.

2.3) Are there many levels of optimization. What are the differences between
these levels?

Example:

2.2) What will be produced by gcc for the program below?
```c
#include <stdio.h>
int main() {
  int i = 7;
  int* p = &i;
  *p = 13;
  printf("The value of i = %d\n", i);
}
Compare it with what gcc does for:
#include <stdio.h>
int main() {
  printf("The value of i = %d\n", 13);
}
```
<!-- ** -->

E.g:
```
compile both with -s, and then -s -O1
```

3) Are all the assembly programs the same?

3.1) A program written in C compiles to the same assembly as a program written in SML?

3.2) When are assembly programs different?
    - When we compile to different computer architectures.

3.3) What is a computer architecture?
- Hardware specification
- Instruction set.

3.4) Give examples of computer architectures.

```
/* In rhyme, at /work/fpereira/classes/lesson4 $ */
$> clang cube.p.c -o cube.p.bc -c -emit-llvm
$> llc cube.p.bc -march=arm -o cube.p.arm.s
$> llc cube.p.bc -march=ppc32 -o cube.p.ppc32.s
$> llc cube.bc -march=mips -o cube.mips.s
```

4) What is the way to execute a program other than compiling?
- With an interpreter.

4.1) Can you write a bash script to list all the files in a director?
```bash
#!/bin/bash
for f in `ls`; do
  echo "File -> $f"
done
```

5) Any other way?
- With a virtual machine.

5.1) What is the difference between an interpreter and a virtual machine?

5.2) What are the advantages of virtual machines?
- portability.
- security.
- profiling.

5.3) What is a famous virtual machine that you know?
- The java virtual machine, which exists in any browser.

5.3.1) How to compile a simple Java program?

5.3.2) What is a '.class'?

To sum it up:
```
Interpreters <-- high-level language
Virtual machines <-- intermediate language
Hardware <-- machine code produced by a compiler
```

6) After linking, the size of a program grows considerably.  
How to avoid this problem?
- Use dynamic linking.

6.1) How does dynamic linking work?

6.2) What is the name of dynamic link libraries in windows?  
.dll

6.3) What about in unix?  
.so

6.4) How is dynamic linking in Java?

6.5) What are the advantages of dynamic libraries?
- multiple programs can share code in memory.
- library code can be updated in separate.
- avoids loading code that is never used.

7) What is a just-in-time compiler?
- compiles while interprets.

8) Where is the implementation of printf in the executable of cube.c?
- Dynamic linking:  
```
$> gcc cube.c -o dyn.exe
$> objdump -x dyn.exe
$> objdump -d dyn.exe
```

- Static linking
```
$> gcc -static cube.c -o static.exe
$> objdump -x static.exe
$> objdump -d static.exe

$> ls *.exe
  7140 dyn.exe
577945 static.exe
```
BINDING

8) In our example program:
```c
int i;
void main() {
  for (i=1; i<=100; i++)
     fred(i);
}
```

What set of values is associated with int?
- language implementation time as in C, or language specification time as in Java.

What is the type of fred?
- compile time.

What is the address of the object code for main?
- load time.

What is the implementation of fred?
- link time.

What is the value of i?
- Runtime.

The binding times are:
Language definition time
Language implementation time
Compile time
Link time
Load time
Runtime

8.1) What is defined during language definition time?
- meaning of key words.

8.2) What is defined during language implementation?
- range of values of int in C (but not in Java)

8.3) What is defined during compilation time?
- type of variables.

8.4) What is defined during link time?
- code of external functions.

8.5) What is defined during load time?
- Memory location of code, data, etc

8.6) What is defined during run time?
- Value of variables.
- Type of variables in Perl, JavaScript, Lisp, etc.

9) What is a debugger?

9.1) What are good debugging informations?
- where is the program executing,
- the trace of execution,
- the value of variables.

Gdb Tutorial:
```
g++ -g buggy.cc
gdb a.out
$> run
$> where
#0  0x08048a32 in Node<int>::next (this=0x0) at buggy.cc:28
#1  0x08048d27 in LinkedList<int>::remove (this=0x804c008,
        item_to_remove=@0xbfc51450) at buggy.cc:77
#2  0x08048969 in main () at buggy.cc:120
$> x 0xbfc51450 // shows whats'up on item_to_remove
```
10.1) Why can't I simply do
```
$> p item_to_remove ?

$> bt
$> break LinkedList<int>::remove
$> condition 1 item_to_remove==1
```

The role of operating system:
- giving memory to the linker,
- taking care of memory management,
- interface with the world: I/O

To sum it up:  
Interpreter: swipl  
Compiler: gcc  
Assembler: as  
Linker: ld  
Loader: operating system.  
Virtual Machine: JVM  
JIT compiler: browser compiling applets.  
