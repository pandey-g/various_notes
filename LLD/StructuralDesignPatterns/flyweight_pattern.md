***<============ move on to the Flyweight Pattern next =============>***

Great 👌 Let’s go step by step through the **Flyweight Pattern**.

---

### 1. **Problem**

Sometimes you need to create **a huge number of similar objects**, which can consume a lot of memory.

Example:

* A text editor might represent each **character** as an object.
* Each character has properties like `font`, `size`, `color`, and also the actual letter (`"a"`, `"b"`, `"c"`, …).
* If you create a new object for every character, you waste memory because many characters share the same formatting.

So the problem: **How can we reduce memory usage when dealing with a large number of similar objects?**

---

### 2. **Solution**

The **Flyweight Pattern** solves this by splitting object state into two parts:

* **Intrinsic state** → shared, invariant data (e.g., font style, color). Stored in flyweights and reused.
* **Extrinsic state** → unique, context-dependent data (e.g., position in the document). Supplied by the client at runtime.

Instead of creating separate objects for each character, you reuse the same flyweight objects (shared), and only pass in the varying data when needed.

---

### 3. **Real-world Intuition / Analogy**

Think of an **airplane seat booking system**:

* The **seat model** (shape, design, color) is the same across many planes.
* Instead of storing duplicate seat details for every single seat, you just store a **shared model** and track the **seat number (extrinsic state)** separately.

---

### 4. **Python Example**

```python
# Flyweight class
class CharacterFlyweight:
    def __init__(self, symbol, font, size):
        self.symbol = symbol      # intrinsic
        self.font = font          # intrinsic
        self.size = size          # intrinsic

    def render(self, position):
        # position is extrinsic (unique per usage)
        print(f"Rendering '{self.symbol}' in {self.font} {self.size}pt at {position}")

# Flyweight Factory
class FlyweightFactory:
    _flyweights = {}

    @classmethod
    def get_flyweight(cls, symbol, font, size):
        key = (symbol, font, size)
        if key not in cls._flyweights:
            cls._flyweights[key] = CharacterFlyweight(symbol, font, size)
        return cls._flyweights[key]

# Client code
factory = FlyweightFactory()

# Instead of creating 3 separate 'a' objects, reuse one flyweight
char1 = factory.get_flyweight("a", "Arial", 12)
char2 = factory.get_flyweight("a", "Arial", 12)
char3 = factory.get_flyweight("b", "Arial", 12)

char1.render((0, 0))
char2.render((10, 0))
char3.render((20, 0))

print("\nTotal unique flyweights created:", len(factory._flyweights))
```

**Output:**

```
Rendering 'a' in Arial 12pt at (0, 0)
Rendering 'a' in Arial 12pt at (10, 0)
Rendering 'b' in Arial 12pt at (20, 0)

Total unique flyweights created: 2
```

Here:

* The **intrinsic state** (`symbol`, `font`, `size`) is shared in flyweights.
* The **extrinsic state** (`position`) is passed at runtime.
* Even though we used `"a"` multiple times, only **one flyweight** was created for `"a"` in Arial 12pt.

---

### 5. **Check-in**

Does the **Flyweight Pattern** make sense with the **airplane seats / text editor characters** analogy and the Python example?
Shall we move on to the **Proxy Pattern** next?
