

# Function Declarations, Definitions, and Prototypes

- **Declaration vs. Definition**: A **declaration** introduces a function's name, return type, and parameter types to the compiler[1][2]. A **definition** provides the actual brace-enclosed function body `{ ... }` that contains executable code[3][4]. C enforces a single-definition rule: every function used must have exactly one definition[5][6].
- **Function Prototypes**: Modern C requires prototypes (e.g., `int power(int base, int n);`)[2][7]. Prototypes allow the compiler to perform strict parameter type-checking and coerce actual arguments to expected types automatically[8].
- **Void Parameters**: To explicitly declare a function that takes no arguments, write `void` inside the parameter list (e.g., `int main(void)`)[12][13]. Leaving parentheses empty `()` represents an old-style declaration that turns off parameter type-checking[8].
# Argument Passing Mechanics

- **Call-by-Value**: All function arguments in C are passed strictly by value[15]. Parameters behave as temporary local variables that receive copies of the caller's argument values; modifying a parameter inside a function does not alter the caller's variable[15].
- **Simulating Call-by-Reference**: To allow a function to modify variables in the calling routine, the caller must pass the addresses of those variables (pointers) using `&amp;`, and the function must dereference those pointer parameters using `*`[18].
- **Array Parameter Rewriting (Decay)**: When an array is passed to a function, it automatically decays to a pointer to its first element[25][26]. In parameter declarations, `int arr[]` is rewritten by the compiler to `int *arr`[27]. Because array length information is lost upon decay, length parameters must be passed explicitly[31].
# Storage Duration, Scope, and Linkage

- **Automatic (Local) Variables**: Variables declared inside a function block have automatic storage duration—they come into existence upon entering the block and disappear when the function exits[34].
- **Internal Static Variables**: Placing the `static` keyword on a local variable preserves its value across successive calls to the function, establishing private, permanent local storage[34].
- **Function Linkage**: Function names are global by default (external linkage) and can be accessed across translation units[37]. Declaring a function `static` limits its visibility strictly to the current source file (internal linkage), preventing naming collisions in multi-file projects[37].
# Advanced & Modern Function Capabilities

- **Function Pointers and Callbacks**: Function names decay into pointers to their executable code[43]. Function pointers can be stored in arrays, passed to other functions, or returned, enabling dynamic callbacks and generic algorithms (such as `qsort`, `bsearch`, or `atexit`)[43].
- **Variadic Functions**: Functions accepting a variable number of parameters (such as `printf`) use the `...` syntax and manage arguments through the `<stdarg.h>` macros (`va_list`, `va_start`, `va_arg`, `va_end`)[7].
- **Inlining and Attributes**: Header-defined `inline` functions allow compilers to eliminate function call overhead by integrating function bodies directly at call sites[55]. Standard attributes like `[[nodiscard]]`, `[[noreturn]]`, `[[unsequenced]]`, and `[[reproducible]]` inform the compiler about return-value requirements, non-returning control flow, and purity for optimization[59].