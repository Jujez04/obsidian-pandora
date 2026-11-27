GCC stands for GNU Compiler Collections which is used to compile mainly C and C++ language. It can also be used to compile Objective C and Objective C++.

The most important option required while compiling a source code file is the name of the source program, rest every argument is optional like a warning, debugging, linking libraries, object file etc.

There are different options in __gcc__ command to allow the user to stop the compilation process at different stages.

```bash
gcc source.c
```
This command produces a `a.out` file which can be compiled by executing with `./a.out` (because it's an executable).
Also it can be explored by the execution of `man` command.

# Options


****-o opt:**** This will compile the source.c file but instead of giving default name hence executed using `./opt`, it will give output file as opt. __-o__ is for output file option.


****-Werror:**** This will compile the source and show the warning if any error is there in the program, -W is for giving warnings.

****-Wall:**** This will check not only for errors but also for all kinds warning like unused variables errors, it is good practice to use this flag while compiling the code.

****-ggdb3:**** This command give us permissions to debug the program using gdb which will be described later, __-g__ option is for debugging.

****-lm :**** This command link __math.h__ library to our source file, -l option is used for linking particular library, for math.h we use -lm.

**-std=c11**: This command will use the c11 version of standards for compiling the __source.c__ program, which allows to define variable under loop initializations also using newer standards version is preferred.

****-c :**** This command compile the program and give the object file as output, which is used to make libraries.

****-v :**** This option is used for the verbose purpose.