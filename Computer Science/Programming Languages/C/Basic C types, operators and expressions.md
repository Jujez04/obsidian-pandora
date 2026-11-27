---
tags:
"created:": 2026-09-08
---

# General Code Structure


~~~C
#include <stdio.h>   // Preprocessing directives for file inclusion

// Constant declarations (with #define)

// static Variable declarations

// Function declarations (Prototypes)

// Function definitions

int main(void)  // Function type + definition (main is the first function to be called)
{
	printf("Hello, world"); // Function call
	// Variable declarations
	// Instructions
	return 0;    // Return statement (optional if function type is void)
}
~~~

# Variable and Function declarations (Best practice)


## Best practice for variables

**Declare Variables Near First Use and Always Initialize**:
- **Avoid delayed initialization:** In older C89 style, all variables had to be declared at the beginning of a block. In modern C, you should define and initialize variables **as close to their first use as possible**. Combining the declaration with its first assignment prevents bugs caused by uninitialized "garbage" values.
- **The Default Initializer:** Under modern standards, if you don't know what concrete value to assign yet, the default initializer `{}` is valid for all types and guarantees a safe state (implicitly setting arithmetic values and pointers to zero or `nullptr`).
- **Use Designated Initializers:** For aggregate types like arrays and structures, prioritize designated initializers (e.g., `.member = value` or `[index] = value`). They make your code highly robust against changes in sizes, positions, or structure layouts.

**Keep Naming and Layout Consistent**
- **Match scope to name length:** Use short, simple names (like `i`, `j` for loop indices) for local variables to prevent cluttering. Use longer, comprehensive, and descriptive names for variables with global or file scope.
- **Bind type modifiers to the left:** Visually separate the type from the identifier by binding modifiers to the left (e.g., write `char* name;` instead of `char *name;`). Additionally, avoid continued declarations on a single line (like `int x, *y;`), as they easily obscure the true type of secondary variables.
- **Do not hide pointers in** **typedefs**: Avoid hiding a pointer behind a `typedef` alias. Keeping the pointer symbol (`*`) visible reminds developers that the variable holds an address, can be dereferenced, and might be `nullptr`.
- **Prefer** **nullptr** **over** **NULL:** Always use `nullptr` for null pointer declarations, as `NULL`'s underlying type is not strictly prescribed and can behave inconsistently across legacy platforms.


## Best practice for functions

**Ensure Prototypes Exist**
- **Prototypes are mandatory:** Modern C standards require all functions to have prototypes. This allows the compiler to perform type-checking and automatically coerce argument types during calls, preventing disastrous mismatches where a separately compiled function is treated with the wrong signature.
- **Explicit empty lists:** If a function takes no arguments, explicitly write `void` (e.g., `int func(void)`). Leaving the parenthesis completely empty (`()`) was used historically as an old-style declaration that turned off parameter checking.

**Be Precise with Array Parameters**
- **Array decay to pointers:** Remember that arrays passed as function parameters automatically rewrite to pointers.
- **Use static sizes in parameters:** To convey to both the reader and compiler that a pointer argument cannot be null and must access a minimum number of elements, use `static` within the parameter brackets (e.g., `void print_val(double arr[static 1])`).
- **Place length parameters first:** For multi-dimensional arrays or variable-length arrays (VLAs), always declare the size parameter _before_ the array parameter so the array can depend on it (e.g., `void func(size_t n, double arr[static n])`).

**Manage Scope, Linkage, and Performance**
- **Prefer** **inline** **over macros:** Whenever you need to optimize a small, simple task, prefer writing an `inline` function over a functional preprocessor macro. Inline functions undergo strict compiler type-checking and avoid macro hazards such as unexpected multiple-evaluation side effects.
- **Limit global space pollution:** Avoid global variables. If a variable or a helper function is private to a single source file, always declare it `static`. This gives it internal linkage, hiding the identifier from other files and preventing naming collisions in larger projects.

## Incorporate Modern C23 Attributes

Use standard attributes in your declarations to enhance safety and help the compiler optimize:
- `[[maybe_unused]]`: Prevents compiler warnings for arguments or variables that might go unused under certain build configurations.
- `[[nodiscard]]`: Tells the compiler to issue a warning if a caller discards the return value—highly recommended for resource allocation or initialization functions where leaks could occur.
- `[[unsequenced]]` (or `[[reproducible]]`): Annotates "pure" functions (functions that have no side effects and only depend on their inputs), giving the optimizer maximum flexibility to combine or parallelize calls.

[[C types]]
[[C operators]]
[[C expressions]]
[[C compilation with GCC compiler]]
