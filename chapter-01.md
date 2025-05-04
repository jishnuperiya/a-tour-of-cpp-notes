# Chapter 1 – The Basics  
## Sections 1.1 and 1.2: Introduction and Programs

---

## 🔹 1.1 Introduction

C++ is a general-purpose programming language designed for:

- High performance (close to hardware)
- Abstraction (classes, templates, custom types)
- Flexibility (procedural, object-oriented, and generic programming)

### 🔧 Structure of a C++ Program

- A C++ program consists of **types** and **functions** spread across source files.
- Each source file is compiled individually into object files.
- All object files are then linked to create a final executable.

You **must have** a `main()` function as the starting point of execution.

---

## 🔹 1.2 Programs

### 📘 Example: The Simplest Program

```cpp
#include <iostream>

int main() {
    std::cout << "Hello, World!\n";
    return 0;
}
```

### ✅ What each part does:

- `#include <iostream>` → Includes the standard output/input library.
- `std::cout` → Standard output stream (console).
- `<<` → Stream insertion operator (sends data to `cout`).
- `\n` → Newline character.
- `return 0` → Indicates success to the OS.

---

### 🔄 Buffering and Flushing Explained

#### 🔹 What is a buffer?

A **buffer** is a temporary memory area where data is stored before being sent somewhere (like the screen or a file). It helps reduce the cost of frequent small operations.

#### 🔹 What is flushing?

**Flushing** means forcing the buffer’s content to be sent immediately to the output destination.

---

### 📌 `\n` vs `std::endl` vs `std::flush`

| Expression              | Action                              | Flushes buffer? |
|-------------------------|-------------------------------------|-----------------|
| `\n`                   | Adds a newline                      | ❌ No           |
| `std::endl`             | Adds newline **and flushes**        | ✅ Yes          |
| `std::flush`            | Just flushes (no newline)           | ✅ Yes          |

#### ✅ Use `\n` when:
- You want a newline without forcing the buffer to flush.
- You're writing in loops or performance-sensitive sections.

#### ⚠️ Use `std::endl` or `std::flush` when:
- You want output to appear **immediately** (like during debugging).
- You need user prompts or log entries to be **flushed instantly**.

---

### 🧪 Example: Performance Comparison

```cpp
for (int i = 0; i < 1000000; ++i)
    std::cout << i << '\n';         // ✅ fast (buffered)

for (int i = 0; i < 1000000; ++i)
    std::cout << i << std::endl;    // ❌ slow (flushes every time!)
```

💡 **Flushing slows down performance** — avoid it unless you need it.

---

### 🔍 When does flushing happen automatically?

- When the buffer **fills up**
- When `std::endl` or `std::flush` is used
- When switching from `std::cout` to `std::cin`
- When the program exits normally

---

### 🚀 Buffering Beyond Console Output

Buffering is not just for console output. It’s crucial in:

- **File I/O**: Writing large files efficiently
- **Networking**: Sending/receiving data in chunks
- **Streaming (audio/video)**: Preventing lag
- **Sensors/embedded systems**: Queuing real-time data
- **Multithreading**: Producer-consumer patterns
- **Graphics**: Double buffering in rendering

Understanding buffering helps write efficient, stable, real-time-capable programs.


## Section 1.3: Functions

---

## 🧠 What are functions?

Functions are named blocks of code that:

- Perform specific tasks
- Promote reuse
- Make code readable and modular

---

## 🔹 Basic Function Structure

```cpp
int add(int a, int b) {
    return a + b;
}
```

- `int` → return type
- `add` → function name
- `(int a, int b)` → parameters
- `return` → gives a result to the caller

---

## 🔹 Function Overloading

C++ allows **multiple functions with the same name** as long as parameter types or counts differ.

```cpp
void print(int i);
void print(double d);
void print(const std::string& s);
```

The compiler picks the correct one based on arguments.

💡 Use only when the functions serve the same logical purpose.

---

## 🔹 Return Types

Functions can return:
- Primitive types: `int`, `bool`, etc.
- Complex types: `std::string`, `std::vector`
- References or pointers

```cpp
std::string greet(const std::string& name) {
    return "Hello, " + name;
}
```

💡 Prefer `const std::string&` to avoid copying large objects.

---

## 🔹 `const&` in Function Parameters

### 🧠 What is `const&`?

- `const T& param` means “**pass a reference** to `param` and **do not modify** it.”
- Used to avoid copying **and** prevent modification.

### ✅ Why use it?

- Speeds up performance by avoiding copies
- Improves safety by preventing accidental modification

### 🔁 Analogy:

| Style                     | Analogy                           |
|---------------------------|-----------------------------------|
| `T param`                 | Make a copy of the parcel         |
| `T& param`                | Hand over the parcel (can change) |
| `const T& param`          | Hand over (read-only)             |

### ⚠️ When NOT to use it:

- For small types like `int`, `char`, `bool`
  ```cpp
  void printAge(int age); // ✅ Better than const int&
  ```

- When you need to modify the input:
  ```cpp
  void change(std::string& text); // Modifies the caller's variable
  ```

---

## 🔹 Default Arguments

You can provide default values:

```cpp
void log(const std::string& msg, bool error = false);
```

- If the second argument is not provided, `false` is used.
- Default values must be **at the end** of the parameter list.

---

## 🔹 Inline Functions

Used for small, frequently-used functions:

```cpp
inline int square(int x) {
    return x * x;
}
```

- Suggests the compiler to replace the call with the body.
- Not guaranteed — it’s only a hint.

---

## 🔹 `auto` Return Type (C++14+)

```cpp
auto multiply(int a, int b) {
    return a * b;
}
```

Useful when the return type is obvious or verbose.  
Can also use trailing return type syntax:

```cpp
auto divide(int a, int b) -> double {
    return static_cast<double>(a) / b;
}
```

---

## ✅ Best Practices

- Use `const&` for read-only large objects (e.g., `std::string`, `std::vector`)
- Use pass-by-value for small types (`int`, `char`)
- Keep functions short and focused
- Avoid modifying inputs unless intended
- Return by value unless there's a performance need

---

## 📌 Summary Table

| Concept              | Best Use                                  |
|----------------------|-------------------------------------------|
| Overloading          | Multiple versions of a logical operation  |
| `const T&`           | For large read-only inputs                |
| `T` by value         | For small types (int, char, etc.)         |
| `T&`                 | If modification of argument is needed     |
| Default Parameters   | Avoids overloads for similar signatures   |
| Inline Functions     | Use for tiny, hot-path functions          |
| `auto` Return Type   | Cleaner and more readable for simple logic|

---

## Section 1.4: Types, Variables, and Arithmetic

---

## 🔹 Fundamental Built-in Types

| Type        | Description                                  |
|-------------|----------------------------------------------|
| `bool`      | Boolean: `true` or `false`                   |
| `char`      | Character (1 byte, often ASCII or UTF-8)     |
| `int`       | Integer                                      |
| `double`    | Floating-point number (64-bit)               |
| `float`     | Smaller floating-point (32-bit)              |
| `unsigned`  | Non-negative integer                         |

### Example:
```cpp
bool is_ready = true;
char letter = 'A';
int age = 30;
double pi = 3.14159;
```

💡 For platform-independent sizes, use `<cstdint>` types like `int32_t`, `uint64_t`.

---

## 🔹 Literals and Number Formats

```cpp
int decimal = 42;
int hex     = 0x2A;
int octal   = 052;
int binary  = 0b101010;  // C++14
```

### 📌 Use Cases:

| Format   | Use Case                                     |
|----------|----------------------------------------------|
| Decimal  | General-purpose programming                  |
| Hex      | Memory, bitmasking, hardware, colors         |
| Octal    | Unix file permissions, legacy compatibility  |
| Binary   | Embedded, bit-level work, unit tests         |

💡 C++14 allows binary literals using `0b`.  
💡 Digit separators (`1'000'000`) make large numbers more readable (C++14+).

---

## 🔹 Variable Declaration and Initialization

```cpp
int a = 5;    // copy initialization
int b{5};     // uniform initialization (preferred)
int c = {5};  // copy-list initialization
```

### ✅ Prefer `{}` because:

- It prevents **narrowing conversions**
- It’s consistent and safer

```cpp
int i1 = 7.8;   // allowed: becomes 7
int i2{7.8};    // ❌ error: narrowing not allowed with {}
```

---

## 🔹 Type Inference with `auto`

```cpp
auto a = 42;       // int
auto b = 3.14;     // double
auto c = "hi";     // const char*
```

💡 Use `auto` when:
- Type is obvious or redundant
- Working with STL iterators or long type names

---

## 🔹 Arithmetic Operators

| Operator | Description             |
|----------|-------------------------|
| `+`      | Addition                 |
| `-`      | Subtraction              |
| `*`      | Multiplication           |
| `/`      | Division (truncates in int) |
| `%`      | Modulo (integers only)   |

```cpp
int a = 10, b = 3;
int q = a / b;  // 3
int r = a % b;  // 1
```

---

## 🔹 Comparison and Logical Operators

| Operator | Description     |
|----------|------------------|
| `==`     | Equal             |
| `!=`     | Not equal         |
| `<`, `>` | Less/greater than |
| `<=`, `>=` | Less/greater or equal |

| Logical | Meaning         |
|---------|-----------------|
| `&&`    | AND              |
| `||`    | OR               |
| `!`     | NOT              |

---

## 🔹 Signed vs Unsigned

```cpp
unsigned int u = 10;
int i = -5;
```

⚠️ Be careful when mixing signed and unsigned values.  
It can lead to **surprising results** due to implicit conversions.

```cpp
unsigned u = 0;
int i = -1;
std::cout << (i < u);  // ❌ might output false!
```

💡 Rule of thumb: avoid mixing signed and unsigned unless necessary.

---

## ✅ Summary Table

| Feature                 | Recommendation                                  |
|-------------------------|--------------------------------------------------|
| `{}` Initialization     | Prefer over `=` to catch narrowing              |
| `auto`                  | Use when type is obvious or long to write       |
| Digit Separators        | Use `'` to improve readability (C++14+)         |
| `int32_t`, `uint64_t`   | Use for platform-independent integer sizes      |
| Signed vs Unsigned      | Be cautious when mixing                         |
| `%` Operator            | Use only for integer modulus                    |

---

## Section 1.5: Scope and Lifetime

---

## 🔹 What is Scope?

Scope defines **where a variable can be accessed** in your program.

### Example:
```cpp
void example() {
    int x = 10;
    {
        int y = 20;
        std::cout << x + y;  // ✅ OK: both x and y are visible here
    }
    std::cout << y;  // ❌ Error: y is out of scope
}
```

### Common Scope Types:

| Scope Type       | Description                              |
|------------------|------------------------------------------|
| Block scope      | Inside `{}`                              |
| Function scope   | Parameters and local variables            |
| Class scope      | Members of classes/structs               |
| Global scope     | Outside any function or class            |
| Namespace scope  | Inside a `namespace`                     |

---

## 🔹 What is Lifetime?

Lifetime describes **how long a variable exists in memory**.

- **Scope**: Where you can see it.
- **Lifetime**: How long it lives.

---

## 🔸 Automatic (Stack) Lifetime

Most local variables live on the stack:

```cpp
void foo() {
    int a = 42;  // Created when foo() starts
}               // Destroyed when foo() ends
```

- Fast and automatically cleaned up

---

## 🔸 Dynamic (Heap) Lifetime

Allocated manually using `new` and `delete`:

```cpp
int* p = new int{42};
// Do something with p
delete p;
```

- You control memory duration
- ⚠️ Prone to leaks and errors
- 💡 Prefer `std::unique_ptr` or `std::shared_ptr`

---

## 🔸 Static Lifetime

Created once and exists until the program ends:

```cpp
static int counter = 0;
```

Useful for stateful behavior across function calls.

---

## ⚠️ Bad Example: Returning a reference to a local variable

```cpp
int* dangerous() {
    int x = 42;
    return &x;  // ❌ x is destroyed after function ends
}
```

Avoid returning addresses or references to local variables.

---

## ✅ Best Practices

- Declare variables **as close to use as possible**
- Use **automatic storage** unless you need dynamic memory
- Avoid `new` and `delete` — use smart pointers
- Never return a reference or pointer to a local variable

---

## 🧾 Summary Table

| Concept   | Meaning                                             |
|-----------|-----------------------------------------------------|
| Scope     | Where variable is visible                           |
| Lifetime  | How long variable stays in memory                   |
| Stack     | Local, fast, auto-cleaned                           |
| Heap      | Manual control, use smart pointers                  |
| Static    | Exists for program duration                         |

---

## Section 1.6: Constants

---

## 🔹 `const` – Runtime constant

```cpp
const double pi = 3.14159;
```

- Value is fixed **after initialization**
- Evaluated at **runtime**
- Can be initialized with runtime expressions

### Example:
```cpp
int input = get_user_input();
const int max_allowed = input + 10;
```

---

## 🔹 `constexpr` – Compile-time constant (C++11+)

```cpp
constexpr int max_size = 100;
```

- Value must be known **at compile-time**
- Useful for:
  - Array sizes
  - Template arguments
  - `switch` case labels

### Example:
```cpp
constexpr int size = 5;
int arr[size];  // OK
```

---

## 🔸 `const` vs `constexpr`

| Feature       | `const`                         | `constexpr`                           |
|---------------|----------------------------------|----------------------------------------|
| Evaluated     | Runtime                         | Compile-time                            |
| Dynamic input | ✅ Allowed                      | ❌ Not unless input is constexpr         |
| In switch     | ❌ Often not usable              | ✅ Fully usable                         |
| Array size    | ❌ May not work                  | ✅ Always works                         |

---

## 🔹 Const Pointers

```cpp
const int* p = &x;     // pointer to const int
int* const p2 = &x;    // const pointer
const int* const p3 = &x; // both pointer and data are const
```

| Declaration           | What’s const?             |
|------------------------|---------------------------|
| `const int* p`         | The data (`*p`)           |
| `int* const p`         | The pointer address       |
| `const int* const p`   | Both data and pointer     |

---

## 🔹 `constexpr` Functions

```cpp
constexpr int square(int x) {
    return x * x;
}

int arr[square(4)];  // OK
```

- Must contain only valid constexpr logic
- Allows usage in constant expressions

---

## ✅ Best Practices

| Tip                                | Why It Helps                                |
|------------------------------------|----------------------------------------------|
| Use `const` liberally              | Communicates intent and prevents mutation    |
| Use `constexpr` where possible     | Enables compile-time checking and optimization |
| Prefer named constants over literals | Improves readability and maintainability    |
| Use `constexpr auto` when type is long | Combines clarity and conciseness          |

---

## 🧾 Summary Table

| Keyword     | Description                        | When to Use                           |
|-------------|------------------------------------|----------------------------------------|
| `const`     | Read-only, value fixed at runtime  | For safety and clarity                |
| `constexpr` | Value known at compile time        | For performance and correctness       |
| `const int*`| Pointer to constant data           | Prevent data change via pointer       |
| `int* const`| Constant pointer                   | Prevent pointer address change        |

---

## Section 1.7: Pointers, Arrays, and References

---

## 🔹 Pointers

A pointer is a variable that stores the **address** of another variable.

```cpp
int x = 42;
int* p = &x;  // p points to x
```

- `*p`: Dereference – access the value at the address
- `&x`: Address-of – get the address of `x`

### 🧠 Pointer Concepts

| Term         | Meaning                          |
|--------------|----------------------------------|
| `int* p`     | pointer to int                   |
| `*p`         | value pointed to (dereference)   |
| `&x`         | address of x                     |

### Example:
```cpp
int x = 10;
int* p = &x;

std::cout << *p;  // prints 10
*p = 20;
std::cout << x;   // prints 20
```

---

## ⚠️ Null and Dangling Pointers

- `nullptr`: special keyword for a null pointer
- Dangling pointer: points to a destroyed object

```cpp
int* p = nullptr;  // ✅ safe

int* d;
{
    int x = 5;
    d = &x;
} // ❌ x destroyed – d is now dangling
```

---

## 🔹 Arrays

### C-style array:
```cpp
int a[5] = {1, 2, 3, 4, 5};
```

- Fixed size
- No bounds checking
- Size must be known at compile time

### Access:
```cpp
std::cout << a[2];  // 3
```

---

## 🔸 Pointer and Array Relationship

Arrays decay to pointers:
```cpp
int a[5] = {1,2,3,4,5};
int* p = a;        // same as &a[0]
std::cout << *(p+2);  // prints 3
```

⚠️ `sizeof(a)` != `sizeof(p)`  
- `sizeof(a)` is total array size  
- `sizeof(p)` is pointer size (usually 8 bytes)

---

## 🔹 Dynamic Arrays

```cpp
int* arr = new int[10];  // heap allocation
arr[0] = 1;
delete[] arr;            // cleanup!
```

💡 Modern C++: prefer `std::vector<int> arr(10);`

---

## 🔹 References

A reference is an **alias** for an existing variable.

```cpp
int x = 5;
int& ref = x;
ref = 10;  // x is now 10
```

- Must be initialized
- Cannot be null
- Cannot change what it refers to

---

## 🔸 Reference vs Pointer

| Feature             | Reference       | Pointer               |
|---------------------|------------------|------------------------|
| Can be null         | ❌ No            | ✅ Yes                 |
| Can change target   | ❌ No            | ✅ Yes                 |
| Dereferencing needed| ❌ No            | ✅ Yes (`*p`)          |
| Safer to use        | ✅ Yes           | ❌ Risk of bugs        |

---

## ✅ Best Practices

- Use **references** when:
  - You want an alias to something that must exist
  - You don’t want nullability

- Use **pointers** when:
  - You want optional or dynamic memory
  - You need to pass "nothing" (`nullptr`)

- Prefer `std::vector` to dynamic C-style arrays
- Avoid `new/delete` unless absolutely necessary
- Always `delete[]` what you `new[]`

---

## 🧾 Summary

| Concept     | Usage Example                          |
|-------------|-----------------------------------------|
| Pointer     | `int* p = &x;`                         |
| Dereference | `*p = 42;`                             |
| Array       | `int a[5] = {1,2,3,4,5};`              |
| Reference   | `int& r = x;`                          |
| Dynamic     | `new[]` and `delete[]` (or use vector)  |

---


## ✅ Section 1.8 – Tests (Control Flow)

C++ uses **tests** (boolean expressions) in control structures like `if`, `while`, `for`, and `switch`.

---

### 🔹 `if` and `else`

```cpp
if (x > 0) {
    std::cout << "Positive";
} else if (x < 0) {
    std::cout << "Negative";
} else {
    std::cout << "Zero";
}
```

---

### 🔹 `while` and `do-while`

```cpp
while (x > 0) {
    --x;
}

do {
    --x;
} while (x > 0);
```

---

### 🔹 `for` Loops

```cpp
for (int i = 0; i < 5; ++i) {
    std::cout << i << " ";
}

std::vector<int> v = {1, 2, 3};
for (int x : v) {
    std::cout << x;
}
```

Use `const auto&` for read-only access to complex types.

---

### 🔹 `switch` Statement

```cpp
switch (choice) {
    case 1: std::cout << "One"; break;
    case 2: std::cout << "Two"; break;
    default: std::cout << "Other";
}
```

- Use `break` to prevent fall-through
- Can only switch on `int`, `char`, or `enum`

---

### 🔹 Ternary Operator

```cpp
std::string status = (score >= 60) ? "Pass" : "Fail";
```

---

### ✅ Best Practices

- Use `{}` even for one-liners
- Prefer range-for loops for containers
- Use `switch` for clear multi-way branching
- Avoid deeply nested logic – use functions

---

## 🧾 Summary – Section 1.8

| Structure      | Use Case                          |
|----------------|------------------------------------|
| `if/else`      | Conditional execution              |
| `while`        | Loop with pre-check                |
| `do-while`     | Loop runs at least once            |
| `for`          | Counted loop or range-for          |
| `switch`       | Clean handling of discrete options |
| `?:`           | Shorthand conditional expression   |

---

## ✅ Section 1.9 – Pointers and Arrays (Advanced)

This section builds on Section 1.7 with deeper insights.

---

### 🔹 Array Decay to Pointer

When passed to a function, arrays decay to pointers:

```cpp
void print(const int* p, int size);
int arr[] = {1,2,3};
print(arr, 3);  // arr becomes int*
```

---

### 🔹 `sizeof` and Arrays

```cpp
int a[10];
sizeof(a);  // total size: 10 * sizeof(int)

void foo(int* a) {
    sizeof(a);  // ❌ just size of pointer (e.g. 8)
}
```

---

### 🔹 Pointer Arithmetic

```cpp
int* p = a;
*(p + 1);  // accesses second element
```

- Moves by element size
- Allows iteration over array

---

### 🔹 Passing Arrays to Functions

```cpp
void f(int a[]);       // treated as int*
void g(int a[10]);     // also int*

// To keep size, use:
void h(std::array<int, 10>& a);
```

---

### 🔹 Multidimensional Arrays

```cpp
int matrix[3][4];
matrix[1][2] = 5;

// Only first dimension can be left out:
void process(int m[][4]);
```

---

### 🔹 Dynamic Memory and Manual Arrays

```cpp
int* p = new int[3]{10, 20, 30};
delete[] p;
```

- Always match `new[]` with `delete[]`
- Manual memory is dangerous → prefer STL

---

### 🔹 Use `std::vector` or `std::array`

| Type              | When to Use                       |
|-------------------|------------------------------------|
| `std::vector<T>`  | Dynamic arrays                    |
| `std::array<T,N>` | Fixed-size, safer than raw array  |
| `std::span`       | View without owning (C++20+)      |

---

### 🧪 Example – `std::span` (C++20)

```cpp
#include <span>
void print(std::span<int> s) {
    for (int x : s) std::cout << x;
}
```

✅ Works with array, vector, or `std::array`

---

## 🧾 Summary – Section 1.9

| Concept            | Key Point                          |
|--------------------|-------------------------------------|
| Array decay        | `T[]` → `T*` when passed to funcs  |
| `sizeof` accuracy  | Works only in original scope       |
| Pointer arithmetic | Walks through elements, not bytes  |
| Multidim arrays    | Only first size can be flexible    |
| Prefer STL         | `vector`, `array`, `span` safer    |

---

## ✅ Section 1.10 – References (Advanced)

References are aliases to existing variables.

```cpp
int x = 10;
int& ref = x;
ref = 20;  // x becomes 20
```

- Must be initialized
- Cannot be reassigned
- Accesses the original variable directly

---

### 🔹 `const` References

Can bind to **temporaries** (rvalues):

```cpp
void print(const std::string& msg);
print("hello");  // binds to a temporary std::string
```

---

### 🔹 Output Parameters

Use references to return results via arguments:

```cpp
void increment(int& value) {
    value += 1;
}
```

---

### 🔹 Returning References

```cpp
int& front(std::vector<int>& v) {
    return v[0];  // valid reference
}
```

⚠️ Never return a reference to a local variable:

```cpp
int& bad() {
    int x = 10;
    return x;  // ❌ x is destroyed after return
}
```

---

### ✅ Best Practices for References

- Use `const T&` for read-only large types
- Use `T&` to modify arguments
- Avoid returning references unless you're certain the object survives

---

## ✅ Section 1.11 – Type Aliases

Aliases give a new name to a type.

---

### 🔹 `typedef` (Old Style)

```cpp
typedef unsigned int uint;
```

---

### 🔹 `using` (Modern C++)

```cpp
using uint = unsigned int;
```

Preferred over `typedef` in modern C++.

---

### 🔹 Template Type Aliases

```cpp
template<typename T>
using Vec = std::vector<T>;

Vec<int> data;  // same as std::vector<int>
```

- `using` works with templates
- `typedef` does not support templates well

---

### ✅ Best Practices

- Prefer `using` over `typedef`
- Use aliases to simplify long type names
- Use template aliases for generic types

---

## ✅ Section 1.12 – The `nullptr`

---

### 🔹 What is `nullptr`?

Introduced in C++11 to represent null pointers.

```cpp
int* p = nullptr;  // ✅ modern null pointer
```

- Type-safe: it has its own type `std::nullptr_t`
- Better than old `NULL` or `0`

---

### 🔹 Why Not `NULL`?

```cpp
#define NULL 0  // in C
```

Confusing in overloads:

```cpp
void f(int);
void f(char*);
f(0);        // ambiguous
f(nullptr);  // ✅ calls f(char*)
```

---

### ✅ Best Practices for `nullptr`

- Always use `nullptr` (never `NULL` or `0`)
- Safer and unambiguous

---


