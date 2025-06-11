When you make your own class, C++ assumes you want:

- A default constructor

- A destructor

- A copy constructor

- A copy assignment operator

- A move constructor

- A move assignment operator

- An equality (==) and inequality (!=) operator

- A swap function

But if you want to manage resources (e.g., memory, file handles), you must control how copying and moving happen.

🧪 Let's Build a Class the Wrong Way First
```cpp
class Data {
    int* ptr;
public:
    Data(int val) {
        ptr = new int(val);
    }
    ~Data() {
        delete ptr;
    }
};
```
```cpp
Data a(5);
Data b = a; // ❌ shallow copy — double delete crash!
```
By default, b = a just copies the pointer, not the actual memory it points to.

### ✅ Rule of 5: The Correct Way to Write a Resource-Managing Class
```cpp
class Data {
    int* ptr;
public:
    // Constructor
    Data(int val) : ptr(new int(val)) {}

    // Destructor
    ~Data() { delete ptr; }

    // Copy Constructor
    Data(const Data& other) : ptr(new int(*other.ptr)) {}

    // Copy Assignment
    Data& operator=(const Data& other) {
        if (this != &other) {
            *ptr = *other.ptr;
        }
        return *this;
    }

    // Move Constructor
    Data(Data&& other) noexcept : ptr(other.ptr) {
        other.ptr = nullptr;
    }

    // Move Assignment
    Data& operator=(Data&& other) noexcept {
        if (this != &other) {
            delete ptr;
            ptr = other.ptr;
            other.ptr = nullptr;
        }
        return *this;
    }
};
```
### 🔄 swap() — The Friendly Utility
Swap lets you exchange data between two objects:

```cpp
void swap(Data& a, Data& b) noexcept {
    using std::swap;
    swap(a.ptr, b.ptr);
}
```
Why? It's used in:

- Assignment

- Sorting

- std::vector::resize

- Exception-safe programming (copy-and-swap idiom)

### ⚖️ Comparison: == and !=
```cpp
bool operator==(const Data& a, const Data& b) {
    return *(a.ptr) == *(b.ptr);
}

bool operator!=(const Data& a, const Data& b) {
    return !(a == b);
}
```
Used in:

- std::find

- Unit tests

- Assertions

### 📚 Modern Tip: Use = default and = delete
If you don’t want copying:

```cpp
Data(const Data&) = delete;
Data& operator=(const Data&) = delete;
```
If you want to say “use compiler-generated version”:

```cpp
Data(const Data&) = default;
Data& operator=(const Data&) = default;
```
This is clearer and safer than writing empty bodies.

## 📦 Chapter 5: Essential Operations – Summary

This chapter covers how to properly implement copy, move, swap, and comparison operations for user-defined types, especially those managing resources.

| Operation               | Purpose                                                                 |
|-------------------------|-------------------------------------------------------------------------|
| Copy Constructor        | Creates a new object as a copy of an existing one.                      |
| Copy Assignment         | Assigns the contents of one object to another.                          |
| Move Constructor        | Transfers ownership of resources from one object to another.            |
| Move Assignment         | Transfers ownership during assignment, avoiding unnecessary copies.     |
| Destructor              | Frees resources when the object is destroyed.                           |
| `swap()`                | Exchanges contents of two objects efficiently and safely.               |
| `==`, `!=` Operators    | Provides logical comparison between objects.                            |
| `= default`             | Ask compiler to generate a default version of the operation.            |
| `= delete`              | Prevent compiler from generating the operation (e.g., to disable copy). |

### 🛠 When to Implement Manually
- Your class manages resources (heap memory, file handles, etc.)
- You want fine-grained control over copying and moving
- You aim for performance and exception safety

### ✅ Rule of 5 Checklist
If you implement any one of these manually, consider implementing all:
- Copy Constructor
- Copy Assignment Operator
- Move Constructor
- Move Assignment Operator
- Destructor

### 🧠 Pro Tip
Use `=default` or `=delete` to make your intent explicit and avoid subtle bugs.

