***<------------what is polymorphism in python------------------>***
```
Duck Typing is Important
```

Great question! Let’s go step by step and deeply unpack **polymorphism in Python**.

---

# 🔹 1. What is Polymorphism?

The word *polymorphism* comes from Greek:

* **poly** = many
* **morph** = forms

So, **polymorphism means "many forms"**.
In programming, it refers to the ability of different objects (belonging to different classes) to respond to the same function or method call in their own way.

👉 In Python, polymorphism is not tied to strict type rules (like in Java or C++), because Python is a **dynamically typed language**. This means you can use the same function or operator with different types of objects, and Python decides at runtime what to do.

---

# 🔹 2. Types of Polymorphism in Python

There are mainly **two levels** of polymorphism:

### (A) **Built-in (Operator / Function) Polymorphism**

Some operators and built-in functions work differently depending on the data types.

**Example 1: Operator `+`**

```python
print(5 + 10)          # Arithmetic addition → 15
print("Hello " + "World")  # String concatenation → "Hello World"
print([1, 2] + [3, 4])     # List concatenation → [1, 2, 3, 4]
```

The operator `+` has different meanings depending on whether it’s used with integers, strings, or lists.

**Example 2: Built-in function `len()`**

```python
print(len("Python"))   # 6 (string length)
print(len([10, 20, 30]))  # 3 (list length)
print(len({"a": 1, "b": 2}))  # 2 (dictionary keys count)
```

👉 Same function name, different behavior depending on input type.

---

### (B) **User-defined (Class-based) Polymorphism**

We can define polymorphism in our own classes, often using:

* **Method Overriding** (common in inheritance)
* **Duck Typing** (Python-specific flexibility)

---

# 🔹 3. Method Overriding (Runtime Polymorphism)

This happens when a **child class redefines a method** from its parent class with a different implementation.

**Example:**

```python
class Animal:
    def speak(self):
        return "Some sound"

class Dog(Animal):
    def speak(self):
        return "Bark"

class Cat(Animal):
    def speak(self):
        return "Meow"

# Polymorphism in action
animals = [Dog(), Cat(), Animal()]
for a in animals:
    print(a.speak())
```

✅ Output:

```
Bark
Meow
Some sound
```

👉 Same method call `speak()`, but different behavior depending on object type.

---

# 🔹 4. Duck Typing (Pythonic Polymorphism)

Python follows the principle:
**“If it walks like a duck and quacks like a duck, it’s a duck.”**

That means Python doesn’t care about the actual type of an object, as long as it supports the methods/attributes you are using.

**Example:**

```python
class Duck:
    def sound(self):
        print("Quack!")

class Dog:
    def sound(self):
        print("Bark!")

def make_sound(animal):
    animal.sound()  # No need to check type!

make_sound(Duck())  # Quack!
make_sound(Dog())   # Bark!
```

👉 `make_sound()` doesn’t care if the object is a `Duck` or `Dog` — it just calls `.sound()`.

---

# 🔹 5. Polymorphism with Functions

We can also write functions that work with different types of objects (as long as they provide expected methods/behaviors).

**Example:**

```python
def add(a, b):
    return a + b

print(add(10, 20))         # 30 (integers)
print(add("Hi ", "there")) # "Hi there" (strings)
print(add([1], [2, 3]))    # [1, 2, 3] (lists)
```

---

# 🔹 6. Polymorphism with Abstract Classes (Formalized Way)

Python also supports **abstract base classes (ABC)** to enforce that subclasses implement specific methods, which is a more formal way of achieving polymorphism.

**Example:**

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

class Circle(Shape):
    def __init__(self, r):
        self.r = r
    def area(self):
        return 3.14 * self.r * self.r

class Square(Shape):
    def __init__(self, s):
        self.s = s
    def area(self):
        return self.s * self.s

shapes = [Circle(5), Square(4)]
for s in shapes:
    print(s.area())
```

✅ Output:

```
78.5
16
```

👉 Each shape provides its own `area()` method, enforcing polymorphism.

---

# 🔹 7. Summary

* **Polymorphism** = same operation, different behavior depending on object.
* In Python, this is possible because it is dynamically typed.
* Achieved through:

  * Built-in operator/function polymorphism (`+`, `len()`).
  * Method overriding in inheritance.
  * Duck typing (most Pythonic way).
  * Abstract classes (formalized polymorphism).

---

Would you like me to also **contrast polymorphism in Python vs statically typed languages (like Java or C++)**, so you see why Python’s dynamic typing makes it more flexible?
