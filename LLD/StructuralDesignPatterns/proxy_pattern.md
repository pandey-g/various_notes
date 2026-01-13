***< ================== move on to the Proxy Pattern next ================>***

Perfect 👌 Let’s wrap up the **Structural Patterns** with the **Proxy Pattern** step by step.

---

### 1. **Problem**

Sometimes, direct access to an object is **not desirable or possible**.

* The object might be **heavy to create** (e.g., loading a large image or connecting to a remote server).
* You might want to **control access** (e.g., security checks).
* Or you want to **add extra behavior** (e.g., logging, caching) without modifying the object itself.

So the problem: **How do we provide controlled, efficient, or secure access to an object without exposing it directly?**

---

### 2. **Solution**

The **Proxy Pattern** provides a **surrogate/placeholder object** that:

* Implements the same interface as the real object.
* Controls access to the real object.
* Adds additional behavior if needed (lazy initialization, logging, caching, security checks, etc.).

The client interacts with the proxy as if it’s the real object — the proxy decides when/how to forward requests.

---

### 3. **Real-world Intuition / Analogy**

Think of a **credit card**:

* Instead of carrying around a bag of cash (real object), you use a **credit card (proxy)**.
* The credit card represents your money and allows controlled access to it.
* You still “pay,” but the proxy (credit card) manages security, logging, and deferred settlement.

---

### 4. **Python Example**

```python
from abc import ABC, abstractmethod
import time

# Subject interface
class Image(ABC):
    @abstractmethod
    def display(self):
        pass

# Real Subject (heavy object)
class RealImage(Image):
    def __init__(self, filename):
        self.filename = filename
        self._load_from_disk()   # expensive operation

    def _load_from_disk(self):
        print(f"Loading image '{self.filename}' from disk...")
        time.sleep(1)  # simulate heavy operation

    def display(self):
        print(f"Displaying image: {self.filename}")

# Proxy
class ProxyImage(Image):
    def __init__(self, filename):
        self.filename = filename
        self._real_image = None

    def display(self):
        if not self._real_image:
            # Lazy initialization
            self._real_image = RealImage(self.filename)
        print("Proxy controlling access...")
        self._real_image.display()

# Client code
print("Client: Creating proxy...")
image = ProxyImage("photo.png")

print("\nClient: First call to display()")
image.display()  # loads and displays

print("\nClient: Second call to display()")
image.display()  # directly displays, no reload
```

**Output:**

```
Client: Creating proxy...

Client: First call to display()
Loading image 'photo.png' from disk...
Proxy controlling access...
Displaying image: photo.png

Client: Second call to display()
Proxy controlling access...
Displaying image: photo.png
```

Here:

* The **client** uses the proxy as if it’s the real image.
* The **proxy** loads the image only when needed (lazy loading).
* Additional behaviors (like logging, access control) can be added inside the proxy.

---

### 5. **Check-in**

Does the **Proxy Pattern** make sense with the **credit card analogy** and the **lazy image loading Python example**?

👉 Since we’ve now covered all **Structural Patterns** (Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy), do you want to **start the Behavioral Patterns** next (like Strategy, Observer, Command…)?
