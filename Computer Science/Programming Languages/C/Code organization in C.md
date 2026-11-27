Organizing code into multiple files is essential for building scalable, maintainable, and reusable C software. In C, code organization is built around the fundamental separation between an **interface** (what a module does) and its **implementation** (how it does it).

# Separation of Concerns: Header and Source files

- **Header Files (** **.h** **) -> The Interface:** Header files declare what is available for other files (called "clients") to use. They contain function prototypes, type definitions (`typedef`, `struct` declarations), macro definitions, and constants. They should **never contain executable function bodies or variable definitions** (which allocate memory).
- **Source Files (** **.c** **) -> The Implementation:** Source files contain the actual executable code (function definitions) and internal data structures. A source file always includes its own corresponding header file (e.g., `calc.c` includes `"calc.h"`) to ensure that function signatures in the implementation match the declarations in the interface.

## Header Guards

When multiple source files include the same header file or headers include other headers the preprocessor can end up inserting the same header text multiple times, causing duplicate definition compiler errors[10][11]. To prevent this, every header file must be wrapped in a **header guard** using preprocessor directives[10]:

```
#ifndef CALC_H
#define CALC_H

// Declarations, macros, and function prototypes go here
void push(double f);
double pop(void);

#endif // CALC_H

```

_How it works:_ The first time the file is included, `CALC_H` is defined[11]. On any subsequent inclusion during the same compilation unit, `#ifndef CALC_H` evaluates to false, and the preprocessor skips straight to `#endif`[11][13].

## Hiding Implementation Details (Opaque Pointers & `static`)

Good modular design reduces coupling by hiding unnecessary internal details from client code[14].

- **Opaque Pointers (Abstract Data Types):** You can declare a structure pointer in a header file without revealing the structure's fields[15][16]. Clients can pass and store the pointer, but only the `.c` implementation file can dereference it and inspect its internal fields[15][16].

```
// In stack.h (Interface)
typedef struct Stack_T *Stack_T; // Opaque pointer type [15]
Stack_T Stack_new(void);

// In stack.c (Implementation)
struct Stack_T {                 // Concrete definition hidden inside .c [17]
    int count;
    struct elem *head;
};

```

- **Internal Scope with** **static** **:** If a helper function or variable is meant only for internal use within a `.c` file, declare it with the `static` keyword[17]. This gives it **internal linkage**, making it completely invisible to other files and preventing global name collisions[17].


# How Compilation works with multiple files

When you compile a C program made of several source files, the build process occurs in three main stages:

1. **Preprocessing:** The preprocessor expands macros (`#define`) and replaces `#include` directives with the raw contents of the referenced header files.
2. **Compiling:** The compiler parses each preprocessed `.c` file individually into an **object code file** (typically ending in `.o` or `.obj`) containing machine-level assembly instructions.
3. **Linking:** The linker resolves external function references across all `.o` files and standard libraries, stitching them together into a single executable binary (such as `a.out` or `program.exe`).

More information in [[C compilation with GCC compiler]]
# Compiling multiple files with GCC

Suppose you have a project split into four files: `calc.h` (shared header), `main.c`, `stack.c`, and `getop.c`[9].

#### **Method 1: Direct Single-Command Compilation**

You can pass all `.c` source files directly to GCC in one command[23]:

```
gcc -Wall -Wextra -std=c11 main.c stack.c getop.c -o calc

```

- `-Wall -Wextra`: Enables standard compiler warnings to catch potential bugs[24][25].
- `-std=c11`: Specifies the C standard version[24].
- `-o calc`: Specifies the name of the final output executable[25][26].
- _Note:_ You **do not** list `.h` files on the command line; the compiler finds them automatically through the `#include "calc.h"` directives inside the `.c` files[9][22].

#### **Method 2: Separate Compilation (Best Practice for Larger Projects)**

For larger applications, compiling every `.c` file from scratch every time you make a small change becomes slow. Instead, you compile each `.c` file into an individual object file (`.o`) using the `-c` flag, and then link the object files together[23]:

1. **Compile each** **.c** **file into object files (** **.o** **):**

```
gcc -Wall -std=c11 -c main.c    # Produces main.o [24]
gcc -Wall -std=c11 -c stack.c   # Produces stack.o [24]
gcc -Wall -std=c11 -c getop.c   # Produces getop.o [24]

```

1. **Link the object files into the final executable:**

```
gcc main.o stack.o getop.o -o calc [24]

```

If you later modify only `stack.c`, you only need to recompile `stack.c` into `stack.o` and run the linking step again, rather than recompiling `main.c` or `getop.c`[23]. In professional software development, this separate compilation process is usually automated using **[[Makefile and CMake]]** or build tools[27].

