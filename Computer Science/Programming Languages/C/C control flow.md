In C, the primary structures used to direct program execution are **statements** (such as selection, iteration, and jump statements). However, C is an "expression-oriented" language and statements are driven entirely by **controlling expressions**, and several of C's unique operators function as expression-level control flow mechanisms


# Controlling Expressions

## if else
The `if` statement evaluates a scalar controlling expression. If the expression evaluates to any non-zero value (true), the associated statement executes. If it evaluates to zero (false), control passes to the optional `else` clause. In C, the controlling expression is not restricted to booleans; pointers, integers, and floating-point types are all valid scalar conditions.

```c
#include <stdio.h>

int main(void) {
    int score = 75;

    // The controlling expression is (score >= 60)
    if (score >= 60) {
        printf("Pass\n");
    } else {
        printf("Fail\n");
    }

    return 0;
}
```
## while
The `while` statement is an entry-controlled loop. It evaluates its scalar controlling expression *before* executing the loop body. If the expression evaluates to non-zero, the body runs, and the expression is re-evaluated; if it evaluates to zero initially, the loop body never executes.

```c
#include <stdio.h>

int main(void) {
    int counter = 3;

    // Evaluated before each iteration
    while (counter > 0) {
        printf("%d\n", counter);
        counter--;
    }

    return 0;
}
```
## do-while
The `do-while` statement is an exit-controlled loop. The body is guaranteed to execute *at least once* before the controlling expression is evaluated at the end of the loop. If the condition evaluates to non-zero, execution jumps back to the beginning of the loop body.

```c
#include <stdio.h>

int main(void) {
    int input = 0;

    // Body executes before the condition is checked
    do {
        printf("Execution guaranteed at least once (value: %d)\n", input);
        input++;
    } while (input < 1);

    return 0;
}
```
## for
The `for` statement standardizes loop bookkeeping into three expressions separated by semicolons: an initial clause (executed once), a controlling expression (evaluated before each iteration), and an iteration expression (evaluated after each iteration). Leaving the controlling expression empty creates an unconditional infinite loop equivalent to `while(1)`.

```c
#include <stdio.h>

int main(void) {
    // (initialization ; controlling expression ; iteration step)
    for (int i = 0; i < 3; i++) {
        printf("Index: %d\n", i);
    }

    return 0;
}
```

## switch-case
The `switch` statement branches execution to one of several `case` labels based on the value of a controlling expression. Unlike other selection statements, the controlling expression must evaluate to an integer type. Execution falls through subsequent cases sequentially unless terminated by a `break` statement.

```c
#include <stdio.h>

int main(void) {
    char grade = 'B';

    // The controlling expression must yield an integer/char
    switch (grade) {
        case 'A':
            printf("Excellent\n");
            break;
        case 'B':
            printf("Good\n");
            break;
        default:
            printf("Other grade\n");
            break;
    }

    return 0;
}
```
# Operators that control flow 

## Ternary Operator
The conditional operator takes three operands: `condition ? expr1 : expr2`. It introduces a sequence point after evaluating the first expression. Only one of the second or third expressions is evaluated (guaranteed short-circuiting at the expression level), making it an expression-level replacement for simple `if-else` branching.

```c
#include <stdio.h>

int main(void) {
    int a = 10, b = 20;

    // Evaluates 'a > b'; since false, only 'b' is evaluated and returned
    int max = (a > b) ? a : b;

    printf("Max: %d\n", max);
    return 0;
}
```

## Logical Operators
The logical AND (`&&`) and logical OR (`||`) operators enforce strict left-to-right evaluation and short-circuit control flow. If the left-hand operand of `&&` evaluates to zero (false), the right-hand operand is not evaluated. If the left-hand operand of `||` evaluates to non-zero (true), the right-hand operand is skipped entirely. This allows expression-level guarding against invalid operations (such as null-pointer dereferencing).

```c
#include <stdio.h>

int main(void) {
    int *ptr = NULL;

    // ptr != NULL evaluates to 0, preventing *ptr from running (avoids SIGSEGV)
    if (ptr != NULL && *ptr == 10) {
        printf("Matches\n");
    } else {
        printf("Safely skipped dereference\n");
    }

    return 0;
}
```

## Comma operator
The comma operator allows multiple expressions to be evaluated sequentially where only one expression is syntactically expected. It evaluates its left operand, discards the result, establishes a sequence point, and then evaluates the right operand. The result and type of the entire expression are those of the right-most operand.

```c
#include <stdio.h>

int main(void) {
    int x = 0, y = 10;

    // Both expressions update within a single controlling step
    for (x = 0; x < 3; x++, y += 2) {
        printf("x: %d, y: %d\n", x, y);
    }

    return 0;
}
```

# Compile-Time Control Flow Expressions


## Preprocessor Conditionals
Preprocessor conditionals direct control flow before compilation begins. The preprocessor evaluates integer constant expressions (which can include macro expansions and the `defined()` operator) to determine whether specific blocks of source text are emitted into the compilation pipeline or discarded.

```c
#include <stdio.h>

#define BUFFER_SIZE 1024

int main(void) {
    // Evaluated at preprocess time; only the true branch reaches the compiler
#if BUFFER_SIZE > 512
    printf("Configured for high-throughput mode\n");
#else
    printf("Configured for low-memory mode\n");
#endif

    return 0;
}
```

## Generic Selections (C11)
Introduced in C11, `_Generic` provides compile-time type-based branching. It inspects the type of a controlling expression (without evaluating the expression at runtime) and routes the selection to the matching type association or `default` label. This enables type-generic macros without incurring runtime dispatch overhead.

```c
#include <stdio.h>

// Routes compile-time code generation based on operand type
#define print_type(X) _Generic((X), \
    int: printf("Type: int, Value: %d\n", (X)), \
    double: printf("Type: double, Value: %f\n", (X)), \
    default: printf("Type: other\n") \
)

int main(void) {
    int a = 42;
    double b = 3.14;

    print_type(a); // Dispatches to the 'int' branch
    print_type(b); // Dispatches to the 'double' branch

    return 0;
}
```