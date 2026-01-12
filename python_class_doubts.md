```
class A:
    def __init__(self,x):
        self.a = x
    def show(self):
        print("world")
class B(A):
    def __init__(self,x):
        self.b = x
        self.a = super().__init__(" world")
    def show(self):
        print(self.b)
        super().show()
         super().a #NoneType 
var = B("hello")
```
print(var.a.a) #gives me error saying NonType has no attribute called a 

You see when I use super().__init__("world") I don't get error but also when trying to access variables it always says NoneType . 

Can you explain why and classes functionality in python .every concept that is present in python classes concepts like OOPs and every thing but do it in more conversation way . asking me if I wanna go with new topic

Ah, good question! Let’s break it down step by step — not just the error you hit, but also how Python classes and OOP concepts work. I’ll explain conversationally, and along the way I’ll ask if you’d like to go deeper into certain concepts like inheritance, polymorphism, `super()`, etc.

---

### First, the code you wrote:

```python
class A:
    def __init__(self, x):
        self.a = x
    def show(self):
        print("world")

class B(A):
    def __init__(self, x):
        self.b = x
        self.a = super().__init__(" world")  # <--- main issue
    def show(self):
        print(self.b)
        super().show()
        super().a # NoneType issue

var = B("hello")
print(var.a.a)  # error
```

---

### 🔎 What’s happening here?

1. **In class `A`:**

   * `__init__` takes `x` and stores it in `self.a`.
   * So if you do `obj = A("foo")`, then `obj.a == "foo"`.

2. **In class `B`:**

   * You call `super().__init__(" world")`.
     That means: run `A.__init__` on this object (`self`), which sets `self.a = " world"`.
   * BUT you also did:

     ```python
     self.a = super().__init__(" world")
     ```

     `super().__init__()` **doesn’t return anything** → it returns `None`.
     So you overwrite `self.a` with `None`.

   That’s why later, when you try to access `var.a`, it’s `None`, not `" world"`.

✅ Correct way would be:

```python
super().__init__(" world")   # don’t assign it to self.a
```

---

### 🧠 Why does `super().__init__()` return `None`?

Because constructors (`__init__`) in Python always **return `None`**.
They are not supposed to “give back” the object — Python handles object creation before `__init__`.
`__init__` just initializes attributes.

So when you wrote `self.a = super().__init__(" world")`, you literally said:

* "Run parent’s init" (sets `self.a = " world"`), ✔️
* Then replace `self.a` with `None`, ❌

---

### 🤔 What about `super().a` in your `show` method?

This line:

```python
super().a
```

does nothing.

* `super()` gives you a proxy to access methods/attributes from the parent class.
* But `super().a` tries to look up an attribute on the *class object*, not on the instance.

If you want to access the `a` attribute from parent init, you can just use `self.a`.

---

### Corrected version:

```python
class A:
    def __init__(self, x):
        self.a = x
    def show(self):
        print("world")

class B(A):
    def __init__(self, x):
        self.b = x
        super().__init__("world")   # <-- no assignment here
    def show(self):
        print(self.b)
        super().show()
        print(self.a)   # access parent’s variable

var = B("hello")
print(var.a)   # "world"
```

Output:

```
hello
world
world
```

---

Now, before I dive deeper — Python classes have a **lot of OOP concepts** (inheritance, multiple inheritance, `super()`, polymorphism, method resolution order, dunder methods like `__str__`, `__repr__`, `__eq__`, static/class methods, encapsulation, etc.).

Do you want me to continue step-by-step into **all OOP concepts in Python**, starting from the very basics (like object creation, attributes) and going up to **advanced topics like MRO and metaclasses**?
Or would you prefer me to stay focused on **inheritance + super()** for now?
