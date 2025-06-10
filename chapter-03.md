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
```
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
```
## 🔹 6. Error Handling

- learn later

- topics: 

- throwing exception
- catching exception
- standard exceptions
- exception safety with raii
🎯 Real-World Example: Reading a Configuration File

Let’s say your program reads a config file. If the file isn’t found or readable, you want to throw an error — but you don’t want to handle it in every helper function. Instead, you let the exception bubble up to main().


#include <iostream>
#include <fstream>
#include <stdexcept>

// Low-level function that might fail
void readConfigFile(const std::string& filename) {
    std::ifstream file(filename);
    if (!file) {
        throw std::runtime_error("Failed to open config file: " + filename);
    }

    std::string line;
    while (std::getline(file, line)) {
        std::cout << "Config: " << line << '\n';
    }
}

// Mid-level function that calls the file reader
void initializeApplication() {
    readConfigFile("config.txt"); // Could throw
}

// Top-level function (main)
int main() {
    try {
        initializeApplication(); // Let exceptions bubble up here
        std::cout << "App initialized successfully.\n";
    } catch (const std::exception& e) {
        std::cerr << "Startup error: " << e.what() << '\n';
        return 1; // Non-zero return code = error
    }

    return 0;
}


🔁 Flow of Execution
	1.	main() calls initializeApplication()
	2.	initializeApplication() calls readConfigFile()
	3.	readConfigFile() tries to open a file
	•	If the file doesn’t exist ➝ it throws an exception
	4.	The exception bubbles up through the stack and is caught in main()
	5.	Program does not crash, and you print a useful error message

you can also wrire your own exception classes



📘 Chapter 4 – Classes (Overview)

This chapter introduces C++ classes, which are the foundation of object-oriented programming in C++.

🔹 Topics Covered:
	1.	Defining a Class
	2.	Constructors
	3.	Copy and Move
	4.	Operator Overloading
	5.	Member Functions
	6.	Access Control (public, private)
	7.	Struct vs Class
	8.	Inline Functions
	9.	Default Member Initialization
	10.	Aggregate Classes


