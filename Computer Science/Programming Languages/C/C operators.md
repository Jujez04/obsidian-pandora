In C, **operators** specify the computations to be performed on data objects (variables and constants), combining them into **expressions** to produce new values. The C language has a remarkably rich and expressive set of operators.


## Categories of C Operators

**Arithmetic Operators**

- **Binary Operators**: `+` (addition), `-` (subtraction), `*` (multiplication), `/` (division), and `%` (modulus)2.
    - **Integer Division & Modulus**: Integer division **truncates** any fractional part2. The C standard guarantees that if `a/b` is representable, then `(a/b)*b + a%b = a`. Since C99, integer division is strictly required to **truncate toward zero** (making `/` and `%` well-defined for negative values).
    - **The Modulus Restriction**: The `%` operator can **only be applied to integral operands** (e.g., `char`, `int`, `long`); it cannot be applied to `float` or `double`.
    - **Unary Operators**: Unary `+` and `-` have higher precedence than their binary counterparts67. Unary `-` produces the arithmetic negation of its operand.
    - **Unsigned Wrap-Around**: Unsigned integer arithmetic (such as on `size_t`) mathematically operates **modulo** $SIZE\_MAX + 1$. It can never formally overflow; if a value exceeds its maximum limit, it cleanly **wraps around**.

**Relational and Equality Operators**

- **Relational Operators**: `<` (less than), `>` (greater than), `<=` (less than or equal to), `>=` (greater than or equal to).
- **Equality Operators**: `==` (equal to) and `!=` (not equal to).
- **Truth Values**: Relational and equality expressions evaluate to **1** **if the relationship is true and** **0** **if it is false** (returning type `int`). In conditional statements, any non-zero value is treated as true, and exactly zero is false.

**Logical Operators**

- **Logical AND (&&) & Logical OR (||)**: Used to join logical conditions.
- **Logical NOT (!)**: Unary operator that converts any non-zero value to `0`, and zero to `1`.
- **Short-Circuit Evaluation**: Both `&&` and `||` guarantee strict **left-to-right evaluation**. Evaluation **stops immediately** as soon as the truth or falsehood of the expression is known. For example, in `b != 0 && (a/b > 1)`, if `b` is `0`, the second expression is never evaluated, safely preventing a division-by-zero crash.

**Bitwise Operators**

These operators perform direct bit-manipulation and apply **only to integral operands**:

- `&` (bitwise AND): Frequently used to **mask off** specific bits (e.g., `n = n & 0177` keeps only the lowest 7 bits).
- `|` (bitwise inclusive OR): Used to **turn bits on**.
- `^` (bitwise exclusive OR): Sets a bit to `1` only if the corresponding bits in the operands are different.
- `~` (unary one's complement): Inverts all bits (converting `1`s to `0`s and vice versa).
- `<<` & `>>` (shift operators): Shift the bit pattern of the left operand left or right by the number of positions specified by the right operand.
    - Left-shifting (`<<`) fills vacated bits with `0`, which is equivalent to **multiplication by** $2^n$ (in the absence of overflow).
    - Right-shifting (`>>`) an unsigned value always fills vacated bits with `0`. Right-shifting a **signed value** may perform a **logical shift** (filling with `0`s) or an **arithmetic shift** (filling with the sign bit) depending on the hardware platform.

**Increment and Decrement Operators**

C provides `++` (adds 1 to its operand) and `--` (subtracts 1). They can be used in two positions, which yield different values in expressions:

- **Prefix (++n / --n)**: Increments or decrements the variable **before** its value is read/used in the surrounding expression.
- **Postfix (n++ / n--)**: Increments or decrements the variable **after** its current value has been read/used.
    - _Example_: If `n` is `5`, `x = n++` sets `x` to `5` and `n` to `6`. Conversely, `x = ++n` sets `x` to `6` and `n` to `6`.
    - These operators can **only be applied to lvalues** (variables or modifiable locations in memory); an expression like `(i+j)++` is strictly illegal.

**Assignment Operators**

- **Simple Assignment (=)**: Evaluates the right-hand expression and copies its value into the left-hand lvalue.
- **Compound Assignment (op=)**: For any binary operator `@`, writing `E1 @= E2` is shorthand for **E1 = (E1) @ (E2)**, except that the location `E1` is only evaluated once. These include `+=`, `-=`, `*=`, `/=`, `%=`, `<<=`, `>>=`, `&=`, `^=`, and `|=`.
- **Assignment is an Expression**: In C, assignment is an **expression, not a statement**. It yields a value (the value of the left operand after modification) and can be nested. This allows idioms like `nl = nw = nc = 0` (associating right-to-left) or `while ((c = getchar()) != EOF)`.

**Conditional (Ternary) Operator (?:)**

C features a ternary operator that takes three operands: **expr1 ? expr2 : expr3**.

- First, `expr1` is evaluated. If it is non-zero (true), `expr2` is evaluated and serves as the result of the entire expression. Otherwise, `expr3` is evaluated and serves as the result.
- **Lazy Evaluation**: Only one of the second or third expressions is ever evaluated.
- **Type Balancing**: If the two alternative result branches are of different types, the compiler applies standard conversion rules to bring them to a common type. For example, if `f` is a `float` and `n` is an `int`, the result of `(cond) ? f : n` is always of type `float`.

**Structure, Array, and Pointer Operators**

- `&` (unary address-of): Returns the memory address of its operand (which must be an lvalue in memory).
- `*` (unary indirection/dereferencing): Accesses the object or function pointed to by a pointer.
- `[]` (array subscripting): `E1[E2]` is defined by the standard as **identical to** **((E1) + (E2))**. Because pointer addition is commutative, array subscripting is also commutative (meaning `a[i]` and `i[a]` are technically equivalent, though the latter is terrible style).
- `.` (direct member selection): Accesses a member of a structure or union object.
- `->` (indirect member selection): Shorthand used to access a member of a structure or union through a pointer. **p->member** **is identical to** **(\*p).member**.

**The Comma Operator (,)**

- A pair of expressions separated by a comma evaluates **left-to-right**, and **the value of the left expression is completely discarded**.
- The type and value of the entire comma expression are those of the right operand. All side effects of the left expression are guaranteed to finish before the right expression starts.
- _Note_: The commas that separate function arguments or variables in declarations are syntactical separators, **not** comma operators, and do not guarantee left-to-right evaluation order.

**Special Operators**

- **sizeof**: Yields the size of an object or type in bytes (representing an unsigned integer constant of type `size_t`). `sizeof(char)` is guaranteed to be exactly `1`. It does not evaluate its expression operand at runtime unless applied to a Variable-Length Array (VLA).
- **Cast Operator** **(type-name)**: Performs explicit type conversion.
- **New C23 Operators**:
    - `<stdckdint.h>`: Introduces type-generic checked arithmetic macros like `ckd_add`, `ckd_sub`, and `ckd_mul` to perform arithmetic while returning a boolean flag indicating if overflow occurred.
    - `<stdbit.h>`: Type-generic bit operation macros such as `stdc_count_ones` (population count) and `stdc_bit_width`.

---

## Precedence and Associativity Table

Operators are grouped below by **decreasing precedence** (highest priority binds first). Operators on the same line have equal precedence57.

| Precedence      | Operators                                                | Description                                                                                        | Associativity     |
| --------------- | -------------------------------------------------------- | -------------------------------------------------------------------------------------------------- | ----------------- |
| **1 (Highest)** | `()` `[]` `->` `.`                                       | Function call, array subscript, structure pointer, structure member                                | Left to right     |
| **2**           | `!` `~` `++` `--` `+` `-` `*` `&` `(type)` `sizeof`      | Unary logical/bitwise NOT, pre/post inc/dec, unary plus/minus, indirection, address-of, cast, size | **Right to left** |
| **3**           | `*` `/` `%`                                              | Multiplicative arithmetic                                                                          | Left to right     |
| **4**           | `+` `-`                                                  | Additive arithmetic                                                                                | Left to right     |
| **5**           | `<<` `>>`                                                | Bitwise left/right shift                                                                           | Left to right     |
| **6**           | `<` `<=` `>` `>=`                                        | Relational comparisons                                                                             | Left to right     |
| **7**           | `==` `!=`                                                | Equality comparisons                                                                               | Left to right     |
| **8**           | `&`                                                      | Bitwise AND                                                                                        | Left to right     |
| **9**           | `^`                                                      | Bitwise exclusive OR                                                                               | Left to right     |
| **10**          | `\|`                                                     | Bitwise inclusive OR                                                                               | Left to right     |
| **11**          | `&&`                                                     | Logical AND                                                                                        | Left to right     |
| **12**          | `\|`                                                     | Logical OR                                                                                         | Left to right     |
| **13**          | `?:`                                                     | Conditional (ternary)                                                                              | **Right to left** |
| **14**          | `=` `+=` `-=` `*=` `/=` `%=` `<<=` `>>=` `&=` `^=` `\|=` | Assignment operators                                                                               | **Right to left** |
| **15 (Lowest)** | `,`                                                      | Comma operator                                                                                     | Left to right     |

---

## Crucial Rules: Order of Evaluation and Side Effects

One of C's most common traps for beginners involves confusing **operator precedence** with the **order of evaluation**.

**Unspecified Evaluation Order**

Precedence and associativity dictate how operators group, but they **do not specify the order in which the operands themselves are evaluated**. For example, in the expression:

```
x = f() + g();
```

The compiler is free to evaluate `f()` before `g()`, or `g()` before `f()`, or even interleave their instructions. If either function modifies a global variable that the other function depends on, the result of the program becomes **compiler-dependent**.

**Side Effect Hazards**

Increment (`++`/`--`) and assignment operators produce **side effects** (modifying variables in memory). If you use a variable with side effects inside a larger expression where the same variable is read or modified elsewhere, the behavior is often **unspecified** or **undefined**.

- _Bad Code_: `a[i] = i++;` (It is undefined whether the array index is the old or new value of `i`).
- _Bad Code_: `printf("%d %d\n", ++n, power(2, n));` (It is unspecified whether `++n` executes before or after evaluating the arguments of `power`).

**Sequence Points & Sequencing**

To manage side effects safely, the standard defines **sequence points** (or sequential ordering)7071. A sequence point is a gate in execution: all previous evaluations and side effects **must be fully completed** before execution can cross into the next sequence.

Sequence points occur at:
1. The end of a full statement (marked by `;` or `}`).
2. The end of the first operand in short-circuit operators (`&&`, `||`), the conditional operator (`?:`), and the comma operator (`,`).
3. Directly after the evaluation of all function arguments but **before** the function is entered.