***<------------- why do we use __new__ in python class------------------->***
Great question 🙂

In Python, `__new__` is a **special method** responsible for **creating a new instance** of a class.

It’s called **before** `__init__`, and it actually returns the new object that will later be initialized.

---

### Key Points about `__new__`:

1. **Static method-like**:
   It belongs to the class, not the instance. The first argument is the class itself (`cls`), not `self`.

2. **Controls object creation**:
   It decides what object to return — usually an instance of the class, but it can also return an instance of a different class (or even an existing object, e.g., in singletons).

3. **Default behavior**:
   By default, it is inherited from `object.__new__`, which simply allocates memory for a new instance.

4. **Execution order**:

   * `__new__` is called first (creates the object).
   * `__init__` is called next (initializes the object).

---

### Example 1 – Normal Use

```python
class MyClass:
    def __new__(cls, *args, **kwargs):
        print("Creating instance (inside __new__)")
        instance = super().__new__(cls)
        return instance
    
    def __init__(self, value):
        print("Initializing instance (inside __init__)")
        self.value = value

obj = MyClass(10)
```

**Output:**

```
Creating instance (inside __new__)
Initializing instance (inside __init__)
```

---

### Example 2 – Singleton Pattern

```python
class Singleton:
    _instance = None
    
    def __new__(cls, *args, **kwargs):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance

a = Singleton()
b = Singleton()

print(a is b)  # True → both point to same object
```

Here, `__new__` ensures only one object is ever created.

---

### When do you use `__new__`?

* When subclassing immutable types (`int`, `str`, `tuple`, etc.) because their values must be set at creation time (in `__new__`), not initialization (`__init__`).
* When implementing design patterns like **Singleton**, **Factory**, or custom object caching.
* When you need **full control** over how objects are created.

---

⚡Quick summary:

* `__new__` → creates the object.
* `__init__` → initializes the object.

Would you like me to also show you a **real-world case** where `__new__` is absolutely required (like subclassing `str` or `tuple`)?


***<---------- can you example of factory pattern ---------->***

Perfect follow-up 👍
The **Factory Pattern** is exactly the kind of use case where `__new__` shines, because it lets you control *which class* gets instantiated at runtime.

---

### 🏭 Example – Factory Pattern with `__new__`

```python
class Shape:
    def __new__(cls, shape_type, *args, **kwargs):
        if cls is Shape:  # Factory logic only applies on the base class
            if shape_type == "circle":
                return Circle(*args, **kwargs)
            elif shape_type == "square":
                return Square(*args, **kwargs)
            else:
                raise ValueError(f"Unknown shape type: {shape_type}")
        return super().__new__(cls)  # Normal creation for subclasses
    
    def area(self):
        raise NotImplementedError("Subclasses must implement this!")


class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14 * self.radius * self.radius


class Square(Shape):
    def __init__(self, side):
        self.side = side

    def area(self):
        return self.side * self.side


# ✅ Client code
s1 = Shape("circle", 5)   # Actually returns a Circle instance
s2 = Shape("square", 4)   # Actually returns a Square instance

print(type(s1), s1.area())  # <class '__main__.Circle'> 78.5
print(type(s2), s2.area())  # <class '__main__.Square'> 16
```

---

### 🔑 How it works

* When you call `Shape("circle", 5)`, Python calls `Shape.__new__`.
* The factory logic inside `__new__` decides *which concrete subclass* (`Circle` or `Square`) to instantiate.
* To the caller, it looks like they’re just using `Shape`, but they actually get the correct subclass.

---

### ✅ When to use this pattern

* When you want a **single entry point** for object creation.
* When object creation depends on **runtime input** (like a string `"circle"` or `"square"`).
* When you want to **hide subclass details** from the user.

---

👉 Do you want me to also show you the **same Factory Pattern without using `__new__`** (just with a class method), so you can compare why/when `__new__` is better?


***<--------how can I achieve the same using decoratores ? and not using __new__------------->***

Great question 🚀

You don’t **have** to use `__new__` for a factory pattern — you can achieve the same effect with a **decorator-based registry**.
This way, each subclass registers itself, and the factory can look them up dynamically.

---

## 🏭 Factory Pattern using Decorators

```python
# Factory registry (maps keys -> classes)
shape_registry = {}

# Decorator for registering classes
def register_shape(name):
    def decorator(cls):
        shape_registry[name] = cls
        return cls
    return decorator


class Shape:
    def area(self):
        raise NotImplementedError("Subclasses must implement this!")


@register_shape("circle")
class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14 * self.radius * self.radius


@register_shape("square")
class Square(Shape):
    def __init__(self, side):
        self.side = side

    def area(self):
        return self.side * self.side


# Factory function (instead of __new__)
def create_shape(shape_type, *args, **kwargs):
    cls = shape_registry.get(shape_type)
    if not cls:
        raise ValueError(f"Unknown shape type: {shape_type}")
    return cls(*args, **kwargs)


# ✅ Client code
s1 = create_shape("circle", 5)
s2 = create_shape("square", 4)

print(type(s1), s1.area())  # <class '__main__.Circle'> 78.5
print(type(s2), s2.area())  # <class '__main__.Square'> 16
```

---

### 🔑 How it works

* `@register_shape("circle")` automatically adds `Circle` to a global `shape_registry`.
* `create_shape("circle", 5)` looks up `Circle` in the registry and instantiates it.
* No `__new__` is required — the factory logic is handled by decorators + a factory function.

---

### ✅ Pros vs `__new__`

* **Cleaner separation**: `__new__` puts factory logic inside the base class; decorators keep it external.
* **Extensible**: You can register new shapes just by adding a class with a decorator, without touching the factory code.
* **More Pythonic** for plugin-style systems (e.g., registering models, commands, routes, etc.).

---

👉 Would you like me to also show a **real-world example** of this decorator-based factory pattern (like registering different ML models or API handlers)?
