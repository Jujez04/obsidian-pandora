To achieve a comprehensive understanding of basic C I/O, you should focus on these **six core building blocks**:

---

### **1. The Stream Abstraction & Standard Streams**

- **Streams**: Standard C abstracts physical devices and disk files as **text streams** (sequences of lines separated by `\n`) or **binary streams** (sequences of raw bytes)[3][4].
- **Standard Streams**: When execution starts, three streams are automatically opened: `stdin` (standard input), `stdout` (standard output), and `stderr` (standard error)[5][6].
- **Stream Buffering**: Text output is typically **buffered** to minimize physical write operations[7]. Terminal output is often **line-buffered** (flushed when a newline `\n` is encountered), while file output is **fully buffered**[8][9]. You can force buffered data to write immediately using `fflush(fp)`[7][10].

---

### **2. Character-Level I/O**

- **getchar()** **&** **putchar(c)**: Read a single character from `stdin` and write a single character to `stdout`[11][12].
- **The** **int** **Return Type**: Functions like `getchar()` return an `int` rather than a `char` so they can return all valid character byte values plus the negative symbolic constant `EOF` when end-of-file or an error occurs[13][14].
- **General File Equivalents**: `fgetc(fp)` and `fputc(c, fp)` perform character-level I/O on any given `FILE*` stream[15][16]. (Note that `getc` and `putc` are equivalent macros that can evaluate stream arguments multiple times)[17].
- **Pushback with** **ungetc**: `ungetc(c, fp)` pushes a character back onto an input stream, allowing lookahead operations[19][20].

---

### **3. Line-Oriented I/O & Safety**

- **fgets(buffer, size, fp)**: Reads up to `size - 1` characters into an array, retaining the newline character `\n` if read, and always appends a terminating null character `\0`[15][21]. It provides built-in bounds checking to prevent buffer overflows[21][22].
- **fputs(str, fp)**: Writes a string to a stream without automatically appending a newline[23][24].
- **The Danger of** **gets()**: The legacy `gets()` function does not accept a maximum buffer size and is inherently vulnerable to security exploits and buffer overflows[25][26]. It has been completely removed from modern C standards[26].

---

### **4. Formatted Output (** **printf** **,** **fprintf** **,** **sprintf** **,** **snprintf** **)**

- **Format Specifications**: Composed as `%[flags][width][.precision][modifier]specifier`[27][28].
- **Specifiers**: Common specifiers include `%d`/`%i` (signed integer), `%u` (unsigned integer), `%x`/`%X` (hexadecimal), `%f`/`%e`/`%g` (floating-point), `%c` (character), `%s` (string), and `%p` (pointer address)[29].
- **Type Modifiers**: Length modifiers like `h` (`short`), `l` (`long`), `ll` (`long long`), `z` (`size_t`), `t` (`ptrdiff_t`), and `L` (`long double`) must match argument types exactly to avoid undefined behavior[34][35].
- **In-Memory Formatting**: While `sprintf` formats text into a character array, it lacks buffer limits[36][37]. **snprintf(buf, size, format, ...)** should always be preferred because it bounds written output to `size` bytes and guarantees null-termination[37].

---

### **5. Formatted Input (** **scanf** **,** **fscanf** **,** **sscanf** **)**

- **Pointer Arguments**: Because C uses call-by-value, arguments passed to `scanf` must be pointers (e.g., `&amp;var`) so the function can write converted values back to the caller's memory[40].
- **Matching & Return Value**: `scanf` parses input characters against the format string and returns the number of successfully assigned items, or `EOF` if input terminates before matching[43][44].
- **Whitespace Traps**: Numeric specifiers like `%d` and `%f` automatically skip leading whitespace[45][46]. However, `%c` and `%[...]` do **not** skip leading whitespace unless you explicitly include a space in the format string (e.g., `" %c"`)[47].
- **Unbounded** **%s** **Hazard**: Plain `%s` reads non-whitespace characters until space is encountered, which can overflow array buffers[25]. Always specify a maximum width (e.g., `%19s`) when scanning strings[50].

---

### **6. File Management & Error Handling**

- **Opening & Closing**: `fopen(path, mode)` opens a file under access modes such as `"r"`, `"w"`, `"a"`, or `"r+"` (with optional `"b"` for binary streams) and returns a `FILE*` pointer (or `NULL` on failure)[6]. `fclose(fp)` closes the stream and flushes buffers[53][54].
- **Checking Errors**: Do not rely on return values alone; use `ferror(fp)` to test for stream write/read errors, and `feof(fp)` to test for end-of-file conditions[55][56]. Remember that `feof(fp)` only evaluates to true **after** a read attempt has failed[57].