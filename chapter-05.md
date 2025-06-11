# Chapter 4:essential operations

## Part 1: essential operations
covers 6 key functions every class may need:

- default constructor
- parametrized constructor
- copy ctor
- move ctor
- copy assignment operator
- move assignment operator
- destructor

we will explore:

- why the compiler auto generates them
- when you should define/delete them
- rule of zero, rule of five, rule of three

## Part 2: Copy and Move Semantics
You’ll learn:

What a memberwise copy is and why it fails for classes with raw pointers.

How to properly implement deep copy for containers (e.g., Vector example).

How to move objects instead of copying to improve performance.

You'll write your own Vector class with all five operations and test them.

## 🔹 Part 3: Resource Management (RAII)
We’ll dig into:

How constructors/destructors are tied to resource ownership.

Why RAII is better than garbage collection.

Real examples using unique_ptr, thread, and how to move them safely.

## 🔹 Part 4: Conventional Operations
We'll explore:

Writing ==, !=, <, etc., in a way the STL expects.

How and why to implement begin(), end(), and size() like STL containers.

Operator overloading for I/O (<<, >>) and user-defined literals.

Writing swap() and hash<>.

## 🔹 Part 5: Advice Recap
We'll conclude with best practices, like:

Always use explicit for single-argument constructors.

Return by value and rely on move.

Avoid memory leaks via RAII and smart pointers.


# 🧱 Part 1: Essential Operations (A Tour of C++ - Chapter 5)

In C++, every class can (and often should) define a consistent set of functions to control how objects are created, copied, moved, assigned, and destroyed.

These are called **special member functions**:

| Operation               | Function Name                  | Purpose                                |
|-------------------------|--------------------------------|----------------------------------------|
| Default Constructor     | `X()`                          | Create a default instance              |
| Parameterized Constructor | `X(args)`                   | Create with arguments                  |
| Copy Constructor        | `X(const X&)`                 | Copy from another object               |
| Copy Assignment         | `X& operator=(const X&)`      | Assign from another object             |
| Move Constructor        | `X(X&&)`                      | Move from another object               |
| Move Assignment         | `X& operator=(X&&)`           | Move and assign                        |
| Destructor              | `~X()`                        | Clean up resources                     |

---

## 🔸 Why Are These Important?

C++ uses these functions in:
- Passing and returning by value
- Container operations
- Resource management (memory, locks, etc.)
- Optimizing performance through move semantics

---

## 🔸 Example: Rule of Five

If your class manages resources (e.g., raw pointers), define **all five**:

```cpp
class MyResource {
    int* data;
public:
    MyResource(int val) : data(new int(val)) {}

    ~MyResource() { delete data; }

    MyResource(const MyResource& other) : data(new int(*other.data)) {}

    MyResource& operator=(const MyResource& other) {
        if (this != &other) {
            delete data;
            data = new int(*other.data);
        }
        return *this;
    }

    MyResource(MyResource&& other) noexcept : data(other.data) {
        other.data = nullptr;
    }

    MyResource& operator=(MyResource&& other) noexcept {
        if (this != &other) {
            delete data;
            data = other.data;
            other.data = nullptr;
        }
        return *this;
    }
};

```

 This is the Rule of Five in action.

## 🔸 Rule of Zero
If your class doesn’t manage raw resources, let the compiler do the work:

```cpp
struct Person {
    std::string name;
    int age;
};
```
✅ Rule of Zero: the compiler will generate all functions correctly.

🔸 Delete Unwanted Operations
Prevent misuse by explicitly deleting operations:

```cpp
class NonCopyable {
public:
    NonCopyable() = default;
    NonCopyable(const NonCopyable&) = delete;
    NonCopyable& operator=(const NonCopyable&) = delete;
};
```
✅ Common for types like std::thread, unique_ptr.

🔸 Defaulted Operations
You can explicitly ask the compiler to generate defaults:

```cpp
class Example {
public:
    Example() = default;
    Example(const Example&) = default;
    Example(Example&&) = default;
    Example& operator=(const Example&) = default;
    Example& operator=(Example&&) = default;
    ~Example() = default;
};
```
✅ Use =default for clarity and completeness.

### summary

| Situation                    | Rule                         |
|-----------------------------|------------------------------|
| Has raw pointer?            | Use Rule of Five             |
| Only STL or safe members?   | Use Rule of Zero             |
| Has destructor?             | Consider Rule of Three or Five |
| Should avoid copying?       | Use `=delete` on copy ops    |
| Want compiler to auto-generate? | Use `=default`            |

rule of three is pre c++11. only in c++11 move semantics got introduced and move constructor and move assignment operator got introduced. so rule of three before c++11 and rule of 5 after c++11
