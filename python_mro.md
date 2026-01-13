***<================= how does MRO  works ? =================>***
Perfect, let’s go deep into **MRO (Method Resolution Order)** — this is one of the trickiest but most elegant parts of Python’s class system.

We’ll break it step by step.

---

## 1. What is MRO?

MRO = **Method Resolution Order**.
It’s the order in which Python searches for an attribute or method when you call it on an object.

Example:

```python
class A:
    def show(self): print("A")

class B(A):
    def show(self): print("B")

obj = B()
obj.show()
```

Here Python looks in order:

```
B → A → object
```

So it finds `show` in **B** and stops.

---

## 2. Where is MRO stored?

Every class has an `__mro__` attribute (a tuple of the search path).

```python
print(B.__mro__)
```

Output:

```
(<class '__main__.B'>, <class '__main__.A'>, <class 'object'>)
```

---

## 3. MRO in Multiple Inheritance

Let’s say:

```python
class A:
    def show(self): print("A")

class C:
    def show(self): print("C")

class B(A, C):
    pass
```

Now:

```python
print(B.__mro__)
```

gives:

```
(<class '__main__.B'>, <class '__main__.A'>, <class '__main__.C'>, <class 'object'>)
```

So if `B` doesn’t have `show`, Python will try `A.show` first, then `C.show`.

---

## 4. The Rule: **C3 Linearization**

Python 3 uses the **C3 Linearization Algorithm** to calculate MRO.
It ensures three things:

1. **Local Precedence Order**

   * The order in which base classes are listed is respected (`class B(A, C)` → `A` before `C`).

2. **Monotonicity**

   * A consistent global order across the hierarchy (so no surprises).

3. **No Skipping Parents**

   * If a class appears multiple times in the hierarchy, it’s used once and in the correct order.

---

## 5. Example with Diamond Inheritance

This is the classic tricky case:

```python
class A:
    def show(self): print("A")

class B(A):
    def show(self): print("B")

class C(A):
    def show(self): print("C")

class D(B, C):
    pass
```

Now what happens when we call:

```python
D().show()
```

Let’s check the MRO:

```python
print(D.__mro__)
```

Output:

```
(<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)
```

👉 So it goes:

```
D → B → C → A → object
```

`D().show()` → **B.show** is chosen.

---

## 6. How does C3 Linearization actually compute that?

The algorithm works like this (simplified):

* Start with the current class.
* Then merge the MROs of parent classes **while preserving their order**.
* Pick the **first valid class** that doesn’t violate order, and continue.

So for `D(B, C)`:

* Start with `D`.
* Merge `B.__mro__ = (B, A, object)` and `C.__mro__ = (C, A, object)` with `(B, C)`.
* Pick `B` first → then `C` → then `A` → then `object`.

---

## 7. Why is `super()` tied to MRO?

`super()` doesn’t mean “go to my parent”.
It means “go to the next class in the MRO”.

So in `D(B, C)`:

* From `D`, `super()` goes to `B`.
* From `B`, `super()` goes to `C`.
* From `C`, `super()` goes to `A`.

This is why **all classes in a cooperative multiple inheritance hierarchy should use `super()`**, not direct parent calls, so the chain works correctly.

---

✅ **Summary:**

* Python searches attributes using MRO.
* MRO is calculated using **C3 linearization**.
* You can inspect it with `Class.__mro__`.
* `super()` follows the MRO chain, not just the immediate parent.

---

⚡ Now, do you want me to:

1. Show you **step-by-step how Python calculates MRO** for a tricky case (like with three or four parents)?
2. Or jump into **practical patterns** (like mixins and why MRO makes them powerful)?
