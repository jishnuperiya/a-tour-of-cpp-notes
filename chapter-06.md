This chapter teaches you how to write template classes, which let you create generic types that work with any data type — like std::vector<T>.

# 🧱 1. What Is a Class Template?
Just like function templates, a class template lets you write one class that works with any type.

## 🧩 Example: Box<T>
```cpp
template<typename T>
class Box {
    T value;
public:
    Box(T v) : value(v) {}

    T get() const { return value; }
    void set(T v) { value = v; }
};
```
You can now do:

```cpp
Box<int> intBox(10);
Box<std::string> strBox("Hello");

std::cout << intBox.get() << std::endl;   // 10
std::cout << strBox.get() << std::endl;   // Hello
```

## 🧰 2. Why Use Templates?
Templates let you:

Write one class for any type (int, double, string, etc.)

Avoid code duplication

Build powerful libraries like STL (Standard Template Library)

## 🧪 3. Class Template With Methods Outside
Sometimes you want to define method bodies outside the class:
```cpp
template<typename T>
class Pair {
    T a, b;
public:
    Pair(T x, T y);
    T sum() const;
};

template<typename T>
Pair<T>::Pair(T x, T y) : a(x), b(y) {}

template<typename T>
T Pair<T>::sum() const {
    return a + b;
}
```
## 🔀 4. Specialization
You can make special versions of the template for specific types.

🧩 Example: Special behavior for bool
```cpp
template<>
class Box<bool> {
    bool value;
public:
    Box(bool v) : value(v) {}
    std::string get() const {
        return value ? "true" : "false";
    }
};
```
## 🧠 5. Template Parameters Can Be Values Too
You can also use values as template parameters:
```cpp
template<typename T, int Size>
class FixedArray {
    T data[Size];
public:
    T& operator[](int i) { return data[i]; }
};
```
Usage:
```cpp
FixedArray<int, 5> arr;
arr[0] = 10;
```
## 🧹 6. typename vs class
```cpp
template<typename T>
template<class T>
```
They are identical. Use whichever you prefer — typename is more modern and consistent.

## 📦 Summary Table

| Concept                      | Meaning                                                  |
|-----------------------------|----------------------------------------------------------|
| `template<typename T>`      | Start of a template class                                |
| `Box<T>`                    | Class using generic type `T`                             |
| `template<>`                | Full specialization for a specific type                 |
| `template<typename T, int N>` | Template with both type and non-type parameters         |
| Method outside class        | Use `template<typename T>` before method definition     |
| `typename` vs `class`       | Both are interchangeable in template declarations       |


## 🛠 Example with main()
```cpp
#include <iostream>
#include <string>

template<typename T>
class Box {
    T value;
public:
    Box(T v) : value(v) {}
    T get() const { return value; }
    void set(T v) { value = v; }
};

int main() {
    Box<int> intBox(42);
    Box<std::string> strBox("Template");

    std::cout << intBox.get() << "\n";
    std::cout << strBox.get() << "\n";

    intBox.set(100);
    std::cout << "Updated intBox: " << intBox.get() << "\n";

    return 0;
}
```