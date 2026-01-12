***<=============== move on to the Observer Pattern next ===============>***
Great! Let’s continue with the **Observer Pattern** 🚀

---

### 2. Observer Pattern

---

### 1. Problem

Sometimes you have a situation where **one object changes state**, and **many other objects need to be updated automatically**.
You don’t want to tightly couple these objects (i.e., the subject should not need to know details about its dependents).

Example:

* A stock market system → When the price of a stock changes, all investors who are interested in that stock should get notified.
* Without this pattern, you’d have to manually call each observer, which is messy and hard to extend.

---

### 2. Solution

The **Observer Pattern** defines a **one-to-many relationship** between objects.

* One object is the **Subject** (the thing being observed).
* Many objects are **Observers** (interested parties).
* When the Subject changes, it **notifies all observers automatically**.

This way, the Subject and Observers are loosely coupled.

---

### 3. Real-World Analogy

Think of **YouTube Subscriptions**:

* The **YouTuber** is the **Subject**.
* The **Subscribers** are the **Observers**.
* Whenever the YouTuber uploads a new video, all subscribers get notified automatically.
* The YouTuber doesn’t need to know *who exactly* the subscribers are, just that they’ll be notified.

---

### 4. Python Example

Let’s implement the **Stock Market example**:

```python
from abc import ABC, abstractmethod

# Observer Interface
class Observer(ABC):
    @abstractmethod
    def update(self, price):
        pass

# Concrete Observers
class Investor(Observer):
    def __init__(self, name):
        self.name = name

    def update(self, price):
        print(f"{self.name} notified: Stock price changed to {price}")

# Subject
class Stock:
    def __init__(self):
        self._observers = []
        self._price = None

    def attach(self, observer: Observer):
        self._observers.append(observer)

    def detach(self, observer: Observer):
        self._observers.remove(observer)

    def notify(self):
        for observer in self._observers:
            observer.update(self._price)

    def set_price(self, price):
        self._price = price
        print(f"\nStock price set to {price}")
        self.notify()

# Client Code
if __name__ == "__main__":
    stock = Stock()

    # Add observers
    investor1 = Investor("Alice")
    investor2 = Investor("Bob")

    stock.attach(investor1)
    stock.attach(investor2)

    # Change stock price
    stock.set_price(100)
    stock.set_price(120)

    # Bob unsubscribes
    stock.detach(investor2)

    stock.set_price(90)
```

**Output:**

```
Stock price set to 100
Alice notified: Stock price changed to 100
Bob notified: Stock price changed to 100

Stock price set to 120
Alice notified: Stock price changed to 120
Bob notified: Stock price changed to 120

Stock price set to 90
Alice notified: Stock price changed to 90
```

---

👉 This way, the **Stock** doesn’t need to know *how many* or *which* investors are observing.
It just notifies everyone who subscribed.

---

✅ Did you understand the **Observer Pattern**?
Shall we move on to the **Command Pattern** next?
