## 📌 What is a `struct`?

A `struct` in C++ is a user-defined type that groups related **variables (data members)** together under one name. It helps organize data logically and cleanly — useful for modeling real-world entities like:

- `Point`
- `Car`
- `Book`
- `SensorData`

---

## 🔑 Key Properties

| Feature                         | Description                                            |
|----------------------------------|--------------------------------------------------------|
| Members are public by default    | Unlike `class`, all members are accessible             |
| No constructor required          | Members can be assigned directly                       |
| Can contain functions            | You can define member functions inside the struct      |
| Supports nesting                 | One struct can contain another                         |

---

## ✅ Basic Syntax

```cpp
struct Point {
    double x;
    double y;
};

### Usage:

```cpp
Point p;
p.x = 3.5;
p.y = 4.2;




## Structs with member functions
structs can also have functions that operate on their own members:
```cpp
struct Point{
    double x;
    double y;

    void print(){
        cout << "(" << x << "," << y << ")"<< endl;
    }
    void move(){
        x+=dx;
        y+=dy;
    }
};

### Usage:

```cpp
Point p = {1.0,2.0};
p.move(3.0,2.0);
p.print(); // output: (4.0,1.0)




'''
