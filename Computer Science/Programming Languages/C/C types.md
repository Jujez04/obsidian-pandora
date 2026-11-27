---
"tags:": "[programming_language, C, types]"
"created:": 2026-08-08
---
|**Type**|**Minimum Guaranteed Size**|**Format Specifier**|**Key Behavior & Rules**|
|---|---|---|---|
|`char`|1 byte (`sizeof == 1`)|`%c`|Signedness is machine-dependent.|
|`signed char` / `unsigned char`|1 byte|`%hhd` / `%hhu`|`unsigned char` (0 to 255) is the fundamental byte atom of C memory.|
|`short` / `unsigned short`|16 bits|`%hd` / `%hu`|Use `%hd` for `scanf`.|
|`int` / `unsigned int`|16 bits (commonly 32)|`%d` or `%i` / `%u`|Unsigned arithmetic operates modulo $2^n$ (cleanly wraps, never overflows).|
|`long` / `unsigned long`|32 bits|`%ld` / `%lu`|Target with `%l` modifier.|
|`long long` / `unsigned long long`|64 bits|`%lld` / `%llu`|Target with `%ll` modifier.|
|`_Bool` / `bool`|1 byte|`%d`|Stores only `0` (false) or `1` (true). Evaluates to boolean in conditions.|
|`float`|32 bits|`%f`, `%e`, `%g`|Promotes to `double` in variadic calls without a prototype (e.g., `printf`).|
|`double`|64 bits|`%f` (print), `%lf` (`scanf`)|Standard precision default for `<math.h>` functions.|
|`long double`|Extended precision ($\ge$ 64 bits)|`%Lf`, `%Le`, `%Lg`|Extended floating-point precision.|
|`_Complex`|$2 \times$ base float size|_Via `creal()` / `cimag()`_|Stored as a contiguous 2-element array of real/imaginary parts.|
|`void`|0 bytes (incomplete)|_N/A_|Represents an empty set of values (no return or no parameters).|
### Semantic Aliases (Typedefs)
Semantic aliases are typedefs that have been built for *portability* and *documentation*. Many types vary significantly across different CPU architectures and operating systems, so the program is safe in terms of portability if we are using these kind of variable types. During compilation they are replaced by the real of that variable.
- **`size_t`** (`%zu`, `%zx`): Unsigned integer large enough to represent the size of any object or array index. It's used for sizes and cardinalities.
- **`ptrdiff_t`** (`%td`): Signed integer large enough to store the difference between two pointers.
- **`time_t`**
- **`intN_t` / `uintN_t`** (`%wN` in C23): Fixed-width exact integers from `<stdint.h>` (e.g., `int32_t`, `uint64_t`).
- **`_BitInt(N)`**: C23 bit-precise integer of exact width `N`.

### Derived and Complex Types

**Pointers (`T *`) & Generic Pointers (`void *`)**
- **Memory & Math:** Stores addresses; pointer arithmetic scales automatically by `sizeof(T)`.
- **Printing:** Print via `%p` (cast target to `(void *)`).
- **Generic Pointer (`void *`):** Holds any object address and converts back without explicit cast. _Cannot be dereferenced or used in arithmetic._

**Arrays (`T[N]`)**
- **Decay Rule:** Automatically converts to a pointer to its first element (`&arr[0]`) in expressions, **except** when used with `sizeof`, unary `&`, or string initializers.
- **Function Parameters:** Parameter declarations like `void f(int arr[])` rewrite internally to `void f(int *arr)` (passed by reference behavior).
- **Restrictions:** Cannot be assigned as a whole unit or compared with `==` / `!=`.

**Structures (`struct`) vs. Unions (`union`)**
- **`struct` (Aggregate):**
    - _Layout:_ Members stored sequentially; compiler may insert padding between/after members, but **never at offset 0**.

    - _Passing:_ Passed strictly **by value** (entire memory block is copied).

    - _Operations:_ Supports assignment (`s1 = s2`) and `&`, but **no equality comparison** (`==`).

- **`union` (Overlay):**
    - _Layout:_ All members share offset 0; total size equals the widest member.

    - _Initialization:_ Defaults to initializing only the first declared member.


**Function Pointers, Bit-fields & Enums**
- **Function Pointers (`R (*fn)(Args)`):** Holds executable entry point; bare function names decay to pointers automatically. Must match exact signature.
- **Bit-fields (`Type name : width`):** Packs integers into arbitrary bit sizes inside structs. _Not byte-aligned, so `&` (address-of) is illegal._
- **Enums (`enum`):** Named integer constants starting at `0` and incrementing by `1` by default.