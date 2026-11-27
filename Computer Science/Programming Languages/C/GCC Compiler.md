# What is the GCC compiler and how it works

**GCC** originally stood for the **"GNU C Compiler,"** but it now stands for the **"GNU Compiler Collection."** This name change reflects the fact that the modern version of GCC is capable of compiling programs written in a variety of languages, including **C, C++, Ada, Fortran, Java, and Objective-C**.

GCC is known for checking programs thoroughly for potential errors and standard compliance through several command-line options:

- **`-Wall`**: Causes the compiler to produce warning messages when it detects potential issues or unusual code.
- **`-W`**: Issues additional warning messages beyond those covered by `-Wall`.
- **`-pedantic`**: Issues all warnings required by the C standard, flagging nonstandard features.
- **`-ansi`**: Disables GCC extensions that conflict with standard C.
- **`-std=`**: Specifies the exact version of the C standard you want the compiler to enforce (e.g., `-std=c89`, `-std=c99`, or `-std=c23`).
## How a C program is compiled

![[Pasted image 20260803183707.png]]

1. **Preprocessing**: The preprocessor processes instructions known as **directives** (lines starting with `#`). It serves as an editor to modify the code, include header files (like `<stdio.h>`), and perform macro expansions.
2. **Compiling**: The modified program is then passed to the compiler proper, which translates the code into machine-readable instructions known as **object code** (producing files typically ending in `.o` or `.obj`). This phase involves other sub-phases:
	1. A **compiler *proper*** parses the expanded source code, checking syntax, generating intermediate code and outputs *assembly language*.
	2. The **assembler** reads the emitted assembly language and translates it into machine-specific *object code*.
3. **Linking**: A linker combines the generated object code with any supplementary code required for execution, such as standard library functions (e.g., `printf`), resulting in a final complete executable file (such as `a.out` on Unix-like environments)

## Formal Standards-Defined Translation Phases

According to the C standard, the translation of a C program is formally executed through successive logically distinct phases:

- **Phase 1 (Lexical Translation & Trigraphs)**: Trigraph sequences (representing characters such as `#`, `[`, `{`) are replaced by their single-character equivalents, and line-boundary formatting is handled.
- **Phase 2 (Line Splicing)**: Lines that end with a backslash (`\`) followed immediately by a newline are deleted to splice/join split lines.
- **Phase 3 (Tokenization, Comments & Directives)**: The program is split into white-space separated tokens. Comments are replaced with a single space character. Preprocessing directives are executed, and macros are expanded.
- **Phase 4 (Escape Translation & String Concatenation)**: Escape sequences inside string literals and character constants are translated to their actual values, and adjacent string literals are concatenated.
- **Phase 5 (Translation & Linking)**: The tokenized, preprocessed code is translated into machine instructions, and then linked with other precompiled libraries and translation units to resolve references to external functions and objects.

