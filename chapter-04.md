# Chapter 4: Classes*

this and next 3 chapters focus on oops

this chapter talks about classes
chapter 5- object lifetime and resource management (constructors, destructors, resource management),how to design classes that clean up after themselves (RAII)
chapter 6- templates - template classes, template functions, function objects
chapter 7 - generic programming

all these features helps you to write object oriented code(with classes and hierarchies) and generic code (with template and concepts).

## 🔹 Introduction
Think of a class as a blueprint for creating your own types.

C++ has built-in types like int, double, and char, but when you want to represent real-world things (e.g., a BankAccount, a Car, a StudentRecord), you define your own type using class.

The goal of this chapter and the next few is to help you:

Represent meaningful concepts using classes

Manage resources like memory automatically

Build reusable, readable, and efficient code using object-oriented and generic programming ideas

## 🔸 Concrete Types
A concrete type behaves like a built-in type — it has:

A known, fixed structure (like built-in int)

Constructors to initialize it

Member functions to use it

A destructor to clean up after itself

Example: complex, Vector, string

These types can:

Be placed directly on the stack

Be copied or moved

Manage their own memory safely

### 🧮 An Arithmetic Type
Imagine we want to represent complex numbers like 3 + 4i. We can write:

```cpp

class complex {
    double re, im; // real and imaginary parts

public:
    complex(double r, double i) : re{r}, im{i} {}
    complex(double r) : re{r}, im{0} {}
    complex() : re{0}, im{0} {}

    double real() const { return re; }
    double imag() const { return im; }

    void real(double d) { re = d; }
    void imag(double d) { im = d; }

    complex& operator+=(complex z) {
        re += z.re;
        im += z.im;
        return *this;
    }
};
```
✅ This is like making your own version of int, but for complex numbers.

### 📦 A Container
A container is a class that stores many elements — like a bag that holds doubles, strings, etc.

Example: Vector that holds double values.

```cpp

class Vector {
    double* elem;
    int sz;

public:
    Vector(int s) : elem{new double[s]}, sz{s} {
        for (int i = 0; i < s; ++i) elem[i] = 0;
    }

    ~Vector() { delete[] elem; } // Destructor: clean up
};

```
new allocates memory for double[s]

delete[] deallocates it

This pattern is called RAII: Resource Acquisition Is Initialization

### 🪄 Initializing Containers
C++ gives you two smart ways to fill a container:

Initializer list
Example:

```cpp
Vector v = {1, 2, 3, 4, 5};
push_back()
Add one element at a time:
```
```cpp
v.push_back(7.5);
```
This is how std::vector also works under the hood.

## 🔸 Abstract Types
An abstract type is a blueprint that defines what something should do, but not how.

You make a class abstract by writing at least one pure virtual function:

```cpp
class Shape {
public:
    virtual void draw() const = 0; // pure virtual
};
```
You can’t create objects of abstract classes. You must derive a class from it and implement the function.

that =0 means "i declare this function but i do not implement it here"

### example:shapes
we define a base class like this:
```cpp
class Shape{
public:
    virtual void draw() const =0; // pure virtual function
};
```
now every shape must implement draw.
```cpp
class Circle : public Shape{
public:
    void draw() const override{
        std::cout << "drawing a circle" <<std::endl;
    }
};
```
```cpp
class Square : public Shape{
public:
    void draw() const override{
        std::cout << "drawing a square";
    }
};

```
now you can see

```cpp
void draw_shape(const Shape& s){
    s.draw(); // works for any shape!
}
```
and call:

```cpp
Circle c;
Square s;
draw_shape(c); // draws circle
draw_shape(s); // draws square
```
this is polymorphism in action. using one interface to work with many kinds of derived classes!

## 🔸 Virtual Functions
When you want polymorphism, use virtual.

```cpp
class Shape {
public:
    virtual void draw() const; // virtual function
};
```
This ensures that if you write shape->draw();, the correct version is called — even if you’re using a base class pointer to refer to a derived object.

## 🔸 Class Hierarchies
Used when you have related types and want them to share behavior.

Example:

```cpp
class Shape { ... };
class Circle : public Shape { ... };
class Rectangle : public Shape { ... };
```
#### ✔️ Benefits from Hierarchies
Code reuse

Unified interface

Easier extension

#### 🔁 Hierarchy Navigation
You can store all shapes in a list of Shape* and draw them without knowing if they’re circles or rectangles.

#### ❌ Avoiding Resource Leaks
Use destructors and smart pointers to clean up memory automatically. If your base class has virtual functions, its destructor must be virtual.

 1. Use Destructors to Release Resources
If you write a class that acquires a resource (like memory using new), you must release it in the destructor.

```cpp
class Buffer {
    int* data;
public:
    Buffer(int size) : data(new int[size]) {}
    ~Buffer() { delete[] data; }  // destructor frees memory
};
```
This ensures that whenever the Buffer object is destroyed, the memory is also cleaned up automatically.

This is part of the RAII principle:
👉 Resource Acquisition Is Initialization — get resource in constructor, release it in destructor.

✅ 2. Use Smart Pointers Instead of new/delete
C++ offers smart pointers like std::unique_ptr and std::shared_ptr that automatically clean up memory.

Example with unique_ptr:
```cpp
#include <memory>

class Buffer {
    std::unique_ptr<int[]> data;
public:
    Buffer(int size) : data(std::make_unique<int[]>(size)) {}
    // no destructor needed! memory auto-freed
};
```
You don't need to write a destructor. When Buffer is destroyed, data is automatically deleted.

⚠️ Virtual Destructor in Base Class
If your class is meant to be inherited, and it has virtual functions, always make its destructor virtual.

Why?
So that when you delete a derived object using a base class pointer, the derived destructor is also called.

🔥 Example of a bug:
```cpp
class Base {
public:
    ~Base() { std::cout << "Base destroyed\n"; }
};

class Derived : public Base {
public:
    ~Derived() { std::cout << "Derived destroyed\n"; }
};

Base* p = new Derived;
delete p;  // ❌ Only Base destructor is called, memory leak!
```
Now fix it with a virtual destructor:

```cpp
class Base {
public:
    virtual ~Base() { std::cout << "Base destroyed\n"; }
};

class Derived : public Base {
public:
    ~Derived() { std::cout << "Derived destroyed\n"; }
};

Base* p = new Derived;
delete p;  // ✅ Both Derived and Base destructors called
```
## ✅ Advice
Use classes to model real-world ideas

Prefer RAII to manual new/delete

Make interfaces abstract when needed

Use virtual functions for polymorphism

Avoid raw pointers in modern C++



## Additional Notes
### new: low level manuall allocation
```cpp
MyClass* p = new MyClass(42);
```
- allocates memory manually  on the heap
- returns raw pointer
- you must call delete p; manually or you will have a memory leak
- no exception safety. if something throws between new and delete, you leak memory

## std::make_unique<T>() : Modern RAII Allocation
```cpp
auto p = std::make_unique<MyClass>(42);
```
- allocates memory and wraps it in a std::unique pointer
- automatically calls delete when the pointer goes out of scope (RAII)
- exception safe: you never leak memory even if exception is thrown
- recommended way to allocate heap memory in modern c++ (c++14+)

