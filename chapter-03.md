# Chapter 3: Modularity – *A Tour of C++ (2nd Edition)*

## 🔹 1. Separate Compilation

- Protect headers with:
  ```cpp
  #pragma once
  // or
  #ifndef HEADER_NAME_H
  #define HEADER_NAME_H
  ...
  #endif

## 🔹 2. Modules (C++20)

Introduced as a modern alternative to header files.

### ✅ Advantages:
- **Faster compile times**
- **Better encapsulation**
- **Fewer name clashes**

### 📦 Example:

#### Module Interface (`math.ixx`)
```cpp
export module math;
export int add(int a, int b);

#### Module Implementation (`math.cpp`)
```cpp
module math;
int add(int a, int b) {
    return a + b;
}
```
#### Use of import instead of include:

```cpp
import math
```


####  Problem with #include
The preprocessor literally copies the content of headers into every .cpp file that includes them.

This leads to redundant parsing of the same code across many translation units.


## 🔹 3. strucutered bindings (c++17)

### Syntax
```cpp
auto [var1, var2, ..., varN] = expression;
```
- expression must return a tuple-like object (e.g., std::pair, std::tuple, array, or struct with public members).


### User-defined structs
```cpp
struct Point {
    int x;
    int y;
};

Point pt{5, 7};
auto [x, y] = pt;
```
### tuples or pairs

```cpp
std::pair<int, std::string> p = {1, "apple"};
int id = p.first;
std::string name = p.second;
```
## 🔹 4. Function Return Values and Arguments

- Use const T& for read-only inputs.

- Use value return (T) for small types or when move semantics are useful.

## 🔹 5. Assertions and Contracts

### 🧪 Runtime Checks (Assertions)

Use `assert` to check assumptions during development. If the condition is false, the program aborts.

```cpp
#include <cassert>

int main() {
    int x = 5;
    assert(x > 0); // Passes
    assert(x < 0); // Fails and terminates the program
}
```
-checked at Runtime(debug) - in release mode, the assertions are removed by the compiler
### 🧪 Compile-time Checks (Static Assertions)

Use static_assert to validate conditions at compile time. Useful for type sizes, traits, or template constraints.

```cpp
static_assert(sizeof(int) == 4, "Unexpected int size");

## 🔹 6. Error Handling

- learn later



