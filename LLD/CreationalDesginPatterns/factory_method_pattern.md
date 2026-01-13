***<============move on to the Factory Method pattern next==================>***

Great! Let’s move on to the **Factory Method Pattern** 🚀

---

# 2) Factory Method Pattern

### 1. Problem

Suppose you’re building an application that can process **different types of documents**:

* Word documents
* PDFs
* Spreadsheets

If you write code like this everywhere:

```python
if type == "pdf":
    doc = PdfDocument()
elif type == "word":
    doc = WordDocument()
```

…it spreads **conditional logic** across the codebase.

👉 Problems:

* Hard to maintain (if a new type comes, you must update everywhere).
* Tight coupling between client code and concrete classes.

---

### 2. Solution

The **Factory Method pattern** says:

* Define a **common interface** for creating objects.
* Let **subclasses decide** which concrete object to create.

Instead of writing `if/else` all over, we **delegate object creation** to a factory method.

---

### 3. Intuition (Real-world analogy)

Think of a **logistics company**:

* If you ask for transport, you don’t decide whether it’s a **truck, ship, or plane**.
* The **logistics department (factory)** decides what vehicle to give you.
* You just say, “Deliver this package.”

So, the client only cares about the interface (`Transport.deliver()`), not the exact class.

---

### 4. Python Example

```python
from abc import ABC, abstractmethod

# Product Interface
class Document(ABC):
    @abstractmethod
    def open(self):
        pass


# Concrete Products
class PdfDocument(Document):
    def open(self):
        return "Opening a PDF document."


class WordDocument(Document):
    def open(self):
        return "Opening a Word document."


# Creator (Factory)
class DocumentFactory(ABC):
    @abstractmethod
    def create_document(self) -> Document:
        pass


# Concrete Factories
class PdfFactory(DocumentFactory):
    def create_document(self) -> Document:
        return PdfDocument()


class WordFactory(DocumentFactory):
    def create_document(self) -> Document:
        return WordDocument()


# Client code
def open_document(factory: DocumentFactory):
    doc = factory.create_document()
    print(doc.open())


# Usage
open_document(PdfFactory())   # "Opening a PDF document."
open_document(WordFactory())  # "Opening a Word document."
```

👉 Key insight:

* Client (`open_document`) doesn’t know whether it’s dealing with `PdfDocument` or `WordDocument`.
* It only knows it gets a **Document** via the factory.

---

✅ That’s the **Factory Method pattern**.
👉 Do you understand this one? Should I move on to the **Abstract Factory pattern** next?
