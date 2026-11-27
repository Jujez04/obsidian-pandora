In C, an **expression** is a formulaic combination of operators and operands that computes a value, designates an object or function, or generates a side effect (such as modifying an object or performing input/output) . Every C expression is characterized by its **value**, **type**, and **binary representation**, which are statically determined at compile time .

## Semantic Classifications: Lvalues, Rvalues, and Function Designators

The C standard categorizes expressions based on what they refer to and how they can be manipulated in memory:

- **Lvalues ("locator values")**: An lvalue is an expression that refers to a specific region of storage (an object) in memory.
    - _Examples:_ An identifier of an arithmetic, structure, union, or pointer type is an lvalue . An indirection expression `*E` is also an lvalue.
    - **Modifiable Lvalues**: To be used as the left-hand operand of an assignment operator (`=`), an lvalue must be modifiable . It must not represent an array, have an incomplete type, or be qualified with `const` .
- **Rvalues ("read values")**: An rvalue is any expression that computes a value but does not designate a persistent object in memory, so it has to be on the right side of the assignment. Rvalues cannot be assigned to, and you cannot apply the address-of operator (`&`) to them.
    - _Examples:_ Constants, arithmetic results like `a + b`, and the result of a prefix/postfix increment operation.
    - *Counterexample*: `42 = x` leads to a compilation error.
- **Function Designators**: An expression that has a function type . Except when used as an operand of the unary `&` operator, a function designator is automatically converted (decays) into a pointer to the function .


## The Expression Spectrum: From Primary to Complex

C expressions are built recursively from the most fundamental units to highly complex operations :

### **Primary Expressions**

The starting points of all computations :

- **Identifiers**: Names representing variables, functions, or constants .
- **Constants/Literals**: Fixed values embedded directly in the source code (e.g., `12`, `3.14`) .
- **String Literals**: Automatically yield a static array of characters, which typically decays into a `char*` pointer .
- **Parenthesized Expressions**: `(E)` has the identical value and type as `E`, serving strictly to alter operator grouping .

### **Postfix Expressions**

Operators that apply to the right of an expression, grouping left-to-right :

- **Array Subscripting (E1[E2])**: Syntactic shorthand defined as `*((E1) + (E2))` .
- **Function Calls (E(...))**: Applies the call operator `()` to a function designator or function pointer, evaluating its arguments before entering the function .
- **Member Selection (E.member and E->member)**: `.member` accesses a structure/union member directly . `->member` acts as a shortcut for `(*E).member` when accessing members via pointers .
- **Postfix Increment/Decrement (E++/E--)**: Evaluates to the current value of `E`, then modifies `E` in memory as a side effect .

### **Unary Expressions**

Operators applying to a single operand, grouping right-to-left :

- **Prefix Increment/Decrement (++E / --E):** Modifies the lvalue and evaluates to the _new_ value .
- **Unary Operators (&, \*, +, -,~,!)**: Access addresses (`&`), dereference pointers (`*`), preserve values (`+`), negate arithmetic values (`-`), invert bits (`~`), or logically negate booleans (`!`) .
- **sizeof** **Operator**: Yields the size of an object or type in bytes without evaluating the operand expression (unless it is a Variable-Length Array) .

### **Casts**

- **(type-name) E**: Coerces the value of `E` to the specified target type, mimicking an assignment to an anonymous variable of that type .

### **Binary & Ternary Expressions**

- **Binary Operators**: Cover arithmetic, bit-shifting, comparisons, bitwise logic, and logical connections .
- **Short-Circuit Evaluation (****&&****,** **||****)**: Evaluates the left operand first; if the overall truth or falsehood is determined, the right-hand operand is completely bypassed .
- **Conditional Operator (****E1 ? E2 : E3****)**: Evaluates `E1`. If true, only `E2` is evaluated; otherwise, only `E3` is evaluated .
- **Assignment (****E1 = E2****)**: Evaluates `E2`, converts it to the type of `E1`, copies the value, and yields the final assigned value as the result .
- **Comma Operator (****E1, E2****)**: Guarantees left-to-right sequencing. `E1` is evaluated for side effects, its value is discarded, and the expression yields the value and type of `E2` .

### **Constant Expressions**

Expressions restricted to a subset of operators that can be fully evaluated at compile time (used for case labels, array sizes, and initializers) . They cannot contain assignments, increment/decrement operators, function calls, or comma operators (except within `sizeof`) .

## Type Resolution: Promotions and Conversions

When operands of differing types meet inside an expression, C enforces strict, automated type-merging rules:

**A. Array and Function Decay**

- An expression of type "array of `T`" automatically converts to a "pointer to `T`" pointing to the first element . This decay is bypassed only under `sizeof`, unary `&`, `++`, `--`, or when acting as the left operand of an assignment or member operator .
- A function designator of type "function returning `T`" automatically decays to a "pointer to function returning `T`" .

**B. Integral Promotions**

- Before any arithmetic occurs, small integer types—such as `char`, `short`, `_Bool`, or enumerations (signed or unsigned)—are automatically promoted to `int` (if `int` can represent the full range of values) or to `unsigned int` (if it cannot) .

**C. Usual Arithmetic Conversions**

These conversions yield a common "super" type for binary operators :

1. **Floating-point dominance**: If either operand is `long double`, `double`, or `float`, the other is promoted to match that respective type in decreasing order of precision .
2. **Unsigned vs. Signed comparison hazards**: If both operands are integers but have different signedness, C applies a rank-based resolution .
    - If the unsigned operand has a rank greater than or equal to the signed operand, the signed operand is implicitly converted to the unsigned type .
    - _The Trap:_ Comparing `-1L < 1U` on a system where `long` is wider than `int` yields `1` (true) because `1U` is promoted to a signed `long` . However, comparing `-1` to `1U` of the same width converts `-1` to a massive positive unsigned integer, evaluating the comparison to `0` (false) .

## Sequencing, Side Effects, and Sequence Points

C gives compiler optimizers massive freedom to reorder calculations . Understanding how side effects interact with sequencing is critical to avoiding undefined behavior:

**Unspecified Evaluation Order**

Precedence dictates how operators group, but **not the order in which operands are evaluated** . In the expression `f() + g()`, the compiler may run `f()` first, `g()` first, or even interleave their execution . If either function has side effects (modifying global variables), the outcome of the program is completely compiler-dependent .

**Sequence Points**

To establish ordering, the C standard defines **sequence points** . A sequence point represents a barrier: all evaluations and side effects from preceding expressions must be completely finished before any evaluations in subsequent expressions can begin .

Sequence points occur:

1. At the end of a full statement (at the `;` or closing `}`) .
2. After the evaluation of a function's designator and arguments, but **before** entering the called function .
3. Immediately after evaluating the first operand of short-circuit operators (`&&`, `||`), the conditional operator (`?:`), and the comma operator (`,`) .
4. At the end of a `return` statement .

**The Sequencing Rule of Thumb**

Between two consecutive sequence points, an object's stored value must be modified at most once by an expression . Furthermore, the prior value must be read only to determine the value to be stored .

-  `a[i] = i++;` ➔ **Undefined Behavior** (unsequenced write and read of `i`) .
- `printf("%d %d\n", ++n, power(2, n));` ➔ **Undefined Behavior** (unsequenced side effect on arguments) .

## Modern C (C11 to C23) Expression Mechanics

Modern C standards introduce powerful compile-time expression mechanisms:

**A. Generic Selections (****_Generic****)**

Introduced in C11, generic selections allow compile-time dispatch based on the type of a controlling expression :

```
#define absolute(X) _Generic((X), \
    float: fabsf, \
    long double: fabsl, \
    default: fabs \
)(X)
```

- **The Controlling Expression**: Evaluated solely for its compile-time type (with type qualifiers, arrays, and functions undergoing decay first) .
- **Validity Constraint**: Every single choice branch inside a `_Generic` selection must represent semantically valid code, even if that branch is not selected for compilation .

**B. C23 Type Inference (****auto** **and** **typeof****)**

- **auto**: Allows a variable to infer its type directly from its initialization expression .
- **typeof** **&** **typeof_unqual**: Extracted directly from C++, these operators query the type of an expression at compile time without actually evaluating any runtime side effects . This allows highly robust type compatibility assertions and safe, macro-based value swapping .