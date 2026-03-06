
# Python OOP – Quick Notes

## 1. Class
A class is a **blueprint for creating objects**.

```python
class Car:
    pass
```

---

## 2. Object
An **object is an instance of a class**.

```python
c1 = Car()
```

---

## 3. Constructor (`__init__`)
Runs automatically when an object is created.

```python
class Car:
    def __init__(self, brand):
        self.brand = brand
```

Usage

```python
c = Car("BMW")
```

---

## 4. Instance Variables
Variables that are **unique to each object**.

```python
class Student:
    def __init__(self, name):
        self.name = name
```

---

## 5. Instance Methods
Functions defined inside a class.

```python
class Dog:
    def bark(self):
        print("Dog barking")
```

Usage

```python
d = Dog()
d.bark()
```

---

## 6. Class Variables
Shared by **all objects of the class**.

```python
class Student:
    school = "ABC School"
```

---

## 7. Encapsulation
Hiding internal data using **private variables**.

```python
class Bank:
    def __init__(self, balance):
        self.__balance = balance
```

---

## 8. Inheritance
A class can **inherit features from another class**.

```python
class Animal:
    def speak(self):
        print("Animal speaks")

class Dog(Animal):
    pass
```

---

## 9. Polymorphism
Same method behaves differently depending on the object.

```python
class Dog:
    def sound(self):
        print("Bark")

class Cat:
    def sound(self):
        print("Meow")
```

---

## 10. Abstraction
Hide complex implementation details.

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass
```

---

## Four Pillars of OOP

| Concept | Meaning |
|--------|--------|
| Encapsulation | Data hiding |
| Inheritance | Code reuse |
| Polymorphism | Multiple behaviors |
| Abstraction | Hide complexity |

---

## Important Keywords

| Keyword | Purpose |
|-------|--------|
| `class` | define a class |
| `self` | reference to current object |
| `__init__` | constructor |
| `pass` | placeholder statement |

---

## Simple Example

```python
class Car:

    def __init__(self, brand):
        self.brand = brand

    def drive(self):
        print(self.brand, "is driving")

c = Car("BMW")
c.drive()
```
