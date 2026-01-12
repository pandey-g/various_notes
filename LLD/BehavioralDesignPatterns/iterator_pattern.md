***<================ move on to the Iterator Pattern next ================>***

Awesome 🚀 Let’s move to the **Iterator Pattern**.

---

### 4. Iterator Pattern

---

### 1. Problem

When working with collections (lists, trees, graphs, custom objects), you often need a way to **traverse elements one by one** without exposing the internal representation (array, linked list, etc.).

If you let clients directly handle collection internals, it:

* Breaks encapsulation
* Forces different traversal logic for each collection
* Makes code less reusable

Example: Imagine a library system where you want to iterate through books. If every collection (list, set, dict) requires a different loop, client code becomes messy.

---

### 2. Solution

The **Iterator Pattern** provides a way to:

* Access elements of a collection sequentially
* Without exposing the underlying structure
* Using a common interface (`has_next`, `next`, or Python’s `__iter__`, `__next__`)

This makes traversal **consistent and decoupled** from the collection implementation.

---

### 3. Real-World Analogy

Think of a **TV remote channel button**:

* You don’t care how channels are stored internally.
* You just press “next” to go to the next channel, one by one.
* The remote (iterator) provides a consistent way to access channels without exposing how they’re arranged inside.

---

### 4. Python Example

Let’s implement a **Book Collection** with a custom iterator:

```python
# Iterator
class BookIterator:
    def __init__(self, books):
        self._books = books
        self._index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self._index < len(self._books):
            book = self._books[self._index]
            self._index += 1
            return book
        raise StopIteration

# Aggregate (Collection)
class BookCollection:
    def __init__(self):
        self._books = []

    def add_book(self, book):
        self._books.append(book)

    def __iter__(self):
        return BookIterator(self._books)

# Client Code
if __name__ == "__main__":
    collection = BookCollection()
    collection.add_book("Design Patterns")
    collection.add_book("Clean Code")
    collection.add_book("The Pragmatic Programmer")

    print("Iterating over books:")
    for book in collection:   # Uses our custom iterator
        print(book)
```

**Output:**

```
Iterating over books:
Design Patterns
Clean Code
The Pragmatic Programmer
```

---

👉 Notice:

* The client just uses a `for` loop.
* It doesn’t know whether books are stored in a list, set, or even a database.
* We can later change the collection’s internal structure without changing client code.

---

✅ Did you understand the **Iterator Pattern**?
Shall we move on to the **Mediator Pattern** next?
