***<-------------Explain all the 23 design patterns in groups start with creational followed by structural and then behavioural . follow the below rule while explaining every design pattern **Strict Rules to follow for every design patterns** 0) Explain One design pattern at a time from 23 design patterns. 1) Always start with describing the problem the design patterns solves 2) Then Give me the solutions i.e how the design patterns actually solves the problem 3) Then the intuition(real time analogy) to remember that would help me to remember it. 4) Then an intuitive python code . 5) Always at the end ask me if I understood it and also ask to move on to other design pattern.------------------->***
Perfect 👍 — I’ll go step by step, one pattern at a time, starting with **Creational patterns**.
We’ll begin with **Singleton**, then move to Factory Method, Abstract Factory, Builder, and Prototype.
I’ll strictly follow your rules for each.

---

# 1) Singleton Pattern

### 1. Problem

Sometimes, in a system, you need to ensure that **only one instance of a class exists**.
Examples:

* Database connection pool (you don’t want multiple pools competing).
* Application-wide configuration (all modules should share the same settings).

If multiple objects are created, it could lead to **inconsistency**, **memory waste**, or **race conditions**.

---

### 2. Solution

The **Singleton pattern** ensures that:

* Only **one instance** of the class can ever exist.
* Provides a **global access point** to this instance.

This is achieved by controlling the object creation process and reusing the same object whenever requested.

---

### 3. Intuition (Real-world analogy)

Think of the **Prime Minister of a country**:

* There can only be **one Prime Minister at a time**.
* Whenever you ask, you always refer to **the same Prime Minister**.
* You don’t “new up” a Prime Minister for yourself — you just get the existing one.

That’s exactly what Singleton enforces.

---

### 4. Python Example

```python
class SingletonMeta(type):
    """A metaclass that ensures only one instance exists."""
    _instances = {}

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            instance = super().__call__(*args, **kwargs)
            cls._instances[cls] = instance
        return cls._instances[cls]


class Config(metaclass=SingletonMeta):
    def __init__(self):
        print("Loading configuration...")
        self.settings = {"db": "mongodb://localhost:27017", "debug": True}


# Usage
c1 = Config()
c2 = Config()

print(c1 is c2)  # True -> both are the same instance
```

👉 Output:

```
Loading configuration...
True
```

Notice how the constructor (`__init__`) is called only once.
Any future `Config()` call just gives the same instance.

---

✅ That’s the **Singleton pattern** explained with problem → solution → intuition → code.

👉 Do you understand this one? Should I move on to the **Factory Method pattern** next?

***<=========== How is metadata class , __call__ is being called? and also how is that __call__(cls,...) here cls is Config it should be of SingletonMeta right ? sice it's a class method, please help me with that understanding ======================>***


Beautiful — this is *exactly* the kind of deep intuition that separates “I know what metaclasses are” from **“I truly understand Python’s object model.”**
Let’s go step-by-step with **why** `SingletonMeta.__call__` runs, and **how** `cls` ends up being `Config` — even though it’s a method of `SingletonMeta`.

---

## 🧩 First: “Metaclasses are the classes of classes.”

Normal classes create *instances* (objects).
Metaclasses create *classes themselves.*

So:

| Level   | Who defines whom          | Example                                   |
| ------- | ------------------------- | ----------------------------------------- |
| Level 1 | Metaclass defines a class | `SingletonMeta` → defines → `Config`      |
| Level 2 | Class defines an instance | `Config` → defines → `config1 = Config()` |

---

## 🧠 Step 1. When Python *defines* your class

When you write:

```python
class Config(metaclass=SingletonMeta):
    ...
```

Python does **not** just create a normal class directly.

Instead, it does something like this behind the scenes:

```python
Config = SingletonMeta("Config", (object,), {...class body...})
```

That means:

* Python **calls** the metaclass (`SingletonMeta.__call__`) to build the class object `Config`.
* The **return value** of that call *becomes* the class itself.

✅ So `Config` is *an instance of* `SingletonMeta`.

---

## 🧠 Step 2. When you create an instance of `Config`

Now, when you do:

```python
obj = Config()
```

You might expect that `Config.__call__` is used.

But here’s the twist:

> **Classes don’t handle object creation themselves — their metaclass does.**

Every class in Python is an **instance of some metaclass**.
For normal classes, that metaclass is `type`.
For your class, it’s `SingletonMeta`.

So when you write `Config()`, Python actually does:

```python
SingletonMeta.__call__(Config, *args, **kwargs)
```

---

## 🧠 Step 3. What happens inside `SingletonMeta.__call__`

Let’s see the function signature again:

```python
def __call__(cls, *args, **kwargs):
```

When it’s invoked as part of `Config()`,
Python passes:

* `cls` = `Config` (the class being instantiated)
* `self` isn’t used because this is a *class method of the metaclass*, not an instance method.

So even though the method *belongs to* `SingletonMeta`,
the **argument it receives (`cls`) is the class being instantiated**, i.e., `Config`.

That’s why you see:

```python
print(cls)        # <class '__main__.Config'>
print(type(cls))  # <class '__main__.SingletonMeta'>
```

---

## 🧠 Step 4. Why does this make sense?

It might help to see the analogy with normal classes.

| Level          | Who defines                        | Method                | Argument          | Meaning                    |
| -------------- | ---------------------------------- | --------------------- | ----------------- | -------------------------- |
| Instance level | `Config` defines `__call__`        | `__call__(self, ...)` | `self` = instance | When you call `instance()` |
| Class level    | `SingletonMeta` defines `__call__` | `__call__(cls, ...)`  | `cls` = class     | When you call `Config()`   |

So:

* For **instances**, `self` refers to the *object being called*.
* For **metaclasses**, `cls` refers to the *class being called*.

Both follow the same “callable object” rule in Python:

> If an object defines `__call__`, calling it like `obj()` invokes `obj.__call__()`.

And since **classes are also objects** (instances of their metaclass),
calling a class like `Config()` just calls its metaclass’s `__call__`.

---

## 🧠 Step 5. What does `super().__call__()` do here?

Inside your `SingletonMeta.__call__`, you wrote:

```python
instance = super().__call__(*args, **kwargs)
```

What’s that doing?

It’s calling the *parent metaclass’s* `__call__` method —
and since the parent of `SingletonMeta` is `type`,
it’s basically doing what `type.__call__()` normally does:

1. Calls `cls.__new__(cls, *args, **kwargs)`
2. Calls `cls.__init__(self, *args, **kwargs)`
3. Returns the instance

So your metaclass wraps that normal object-creation behavior to enforce the singleton pattern.

---

## ⚙️ Step 6. Quick summary with arrows

Let’s visualize the chain of calls.

### When defining the class:

```
SingletonMeta.__call__("Config", bases, attrs)
    ↓
creates Config (class)
```

### When creating an instance:

```
SingletonMeta.__call__(Config, *args, **kwargs)
    ↓
calls type.__call__(Config, *args, **kwargs)
    ↓
calls Config.__new__(Config, *args, **kwargs)
    ↓
calls Config.__init__(self, *args, **kwargs)
    ↓
returns instance of Config
```

---

## ✅ So to directly answer your two questions:

> ❓ How is the metaclass’s `__call__` being called?

Because when you call `Config()`, Python looks at `type(Config)` —
that’s `SingletonMeta` —
and calls its `__call__` method.

---

> ❓ Why does `__call__(cls, …)` receive `cls = Config` and not `SingletonMeta`?

Because `__call__` is *bound to the metaclass*, but is *invoked with the class being instantiated* (`Config`) as its argument.
`cls` = the class you’re trying to instantiate (not the metaclass itself).

---

Would you like me to show this same flow visually with a print-tracing example (`__new__`, `__init__`, `__call__` on both metaclass and class) so you can *see the exact order* of calls when running `obj = Config()`?
It’s really eye-opening when you see all 6 methods firing in sequence.


*** <============ class WeaviateClientWrapper:
    """Connects to Weaviate and manages schema."""
    
    _instance = None
    _schema_ensured = False

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super(WeaviateClientWrapper, cls).__new__(cls)
        return cls._instance

How does super(WeaviateClientWrapper, cls).__new__(cls) work ? =======> ***

Excellent question — this line 👇

```python
super(WeaviateClientWrapper, cls).__new__(cls)
```

looks simple but hides one of Python’s most subtle mechanisms:
how class inheritance resolution works for object creation.

Let’s unpack it *visually and conceptually*, step by step.

---

## 🧩 Step 1. What happens when you create an object

When you write:

```python
client = WeaviateClientWrapper()
```

Python calls:

```python
WeaviateClientWrapper.__new__(WeaviateClientWrapper)
```

since `__new__` is the method that **creates (allocates)** the object in memory.

---

## 🧠 Step 2. Why use `super(...)` inside `__new__`

Inside `__new__`, you normally want to **delegate actual object creation** to the base class (`object` by default).

The base class `object.__new__` is the one that actually allocates memory and returns a blank instance.

So if you do this:

```python
instance = object.__new__(cls)
```

That would work.

But `super()` is a cleaner and safer way to find the **next class in the method resolution order (MRO)** —
especially if you later subclass `WeaviateClientWrapper`.

---

## 🧩 Step 3. What `super(WeaviateClientWrapper, cls)` actually means

Let’s break it down:

* `WeaviateClientWrapper` → the current class.
* `cls` → the class we’re currently instantiating (could be a subclass too!).
* `super(WeaviateClientWrapper, cls)` → find the **next class** in the MRO (method resolution order) after `WeaviateClientWrapper`, starting from `cls`.

So if `cls` is `WeaviateClientWrapper`, the MRO is:

```
WeaviateClientWrapper → object
```

Then:

```python
super(WeaviateClientWrapper, cls)
```

returns a **proxy object** that represents the next class in the chain, i.e. `object`.

So when you call:

```python
super(WeaviateClientWrapper, cls).__new__(cls)
```

…it’s actually calling:

```python
object.__new__(WeaviateClientWrapper)
```

✅ which creates the **actual instance in memory.**

---

## 🧠 Step 4. Why use `super()` instead of `object.__new__` directly?

Because if you subclass `WeaviateClientWrapper`, it still works correctly!

Example:

```python
class MyClient(WeaviateClientWrapper):
    pass
```

Now, if you do `MyClient()`, the MRO becomes:

```
MyClient → WeaviateClientWrapper → object
```

and inside the `__new__` method of `WeaviateClientWrapper`,
`cls` = `MyClient`.

So now:

```python
super(WeaviateClientWrapper, cls)
```

returns a proxy for the **next class in the MRO after `WeaviateClientWrapper`** — again `object`.

Thus:

```python
super(WeaviateClientWrapper, cls).__new__(cls)
```

effectively calls `object.__new__(MyClient)` — perfect behavior.

If you had hardcoded `object.__new__(WeaviateClientWrapper)`,
you’d be returning the wrong type of object for subclasses.

---

## 🧠 Step 5. Full flow inside your Singleton

Here’s how your singleton creation works step-by-step:

```python
class WeaviateClientWrapper:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            print("Allocating new instance...")
            cls._instance = super(WeaviateClientWrapper, cls).__new__(cls)
        return cls._instance
```

Now:

```python
a = WeaviateClientWrapper()
b = WeaviateClientWrapper()
```

Output:

```
Allocating new instance...
```

✅ Only the first call creates the object (via `object.__new__(WeaviateClientWrapper)`),
✅ The second call returns the same `_instance`.

---

## 🧩 Step 6. Mental model (simple visualization)

```
WeaviateClientWrapper() 
│
└──> WeaviateClientWrapper.__new__(WeaviateClientWrapper)
        │
        ├──> calls super(WeaviateClientWrapper, WeaviateClientWrapper)
        │       ↓
        │     resolves to object
        │
        └──> object.__new__(WeaviateClientWrapper)
                 ↓
             allocates memory for new instance
```

---

## ✅ TL;DR summary

| Concept                                          | Meaning                                                                       |
| ------------------------------------------------ | ----------------------------------------------------------------------------- |
| `__new__`                                        | Creates (allocates) the object before `__init__` initializes it               |
| `super(WeaviateClientWrapper, cls)`              | Finds the next class in MRO after `WeaviateClientWrapper`                     |
| `super(WeaviateClientWrapper, cls).__new__(cls)` | Calls `object.__new__(cls)` safely (even for subclasses)                      |
| `cls`                                            | The actual class being instantiated (`WeaviateClientWrapper` or its subclass) |

---

If you’d like, I can show you a **trace print example** that logs exactly which class and `__new__` / `__init__` methods are called when subclassing this singleton — it really clarifies how `super()` and `cls` move through the MRO.
Would you like to see that?
