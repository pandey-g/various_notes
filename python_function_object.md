***<==============  what function are truly are ? You said it's object , if so what is the name of the class ? ==============>***
Ooooh, you’ve hit the *fun* part 😎 — in Python, **functions really are objects**, just like everything else. Let’s peel back the curtain.

---

## 1. Functions are objects

When you write:

```python
def foo(x):
    return x + 1
```

Python does 2 things under the hood:

1. Creates a **function object** (of type `function`).
2. Binds it to the name `foo` in the current namespace.

So:

```python
print(type(foo))
```

gives:

```
<class 'function'>
```

That means `foo` is an *instance* of the built-in class **`function`**.

---

## 2. Functions are first-class citizens

Since functions are objects, you can:

* Assign them to variables:

  ```python
  bar = foo
  print(bar(10))   # 11
  ```
* Pass them as arguments:

  ```python
  def call_twice(f, x):
      return f(f(x))

  print(call_twice(foo, 2))  # (2+1)+1 = 4
  ```
* Store them in lists, dicts, etc.

---

## 3. Attributes of function objects

Functions carry metadata. Example:

```python
def foo(x): return x+1

print(foo.__name__)     # "foo"
print(foo.__code__)     # code object with compiled bytecode
print(foo.__defaults__) # default arguments
print(foo.__closure__)  # closures if it captures outer variables
```

So a function is not just code — it’s an object holding code + metadata.

---

## 4. Bound vs Unbound methods

When you access a method from a class or instance, Python creates a **method object** on the fly.

Example:

```python
class C:
    def show(self): print("hi")

print(type(C.show))     # <class 'function'>
print(type(C().show))   # <class 'method'>
```

* `C.show` is a plain function object.
* `C().show` is a **bound method** object — it wraps the function + the instance (`self`).

That’s why calling `obj.show()` automatically passes `obj` as the first argument.

---

## 5. Digging deeper

* The **class** of a normal function is `<class 'function'>`.
* The **class** of a bound method is `<class 'method'>`.
* Both are implemented in C inside Python’s runtime.

---

✅ **Summary so far:**

* Functions are objects → instances of `<class 'function'>`.
* When taken from an instance, they turn into `<class 'method'>`.
* They carry attributes like `__name__`, `__code__`, `__defaults__`.

---

⚡ So, do you want me to **show you exactly how Python creates and binds methods** when you do `obj.show()` → including how the descriptor protocol (`__get__`) makes it work? That’s the deepest level of this rabbit hole 🐇. Would you like to go there?
