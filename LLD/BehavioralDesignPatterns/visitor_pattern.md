***<============== move on to the Visitor Pattern next ===================>***

Great 🚀 Let’s move to the **Visitor Pattern**.

---

### 9. Visitor Pattern

---

### 1. Problem

Sometimes, you have a complex object structure (like a tree, document, or AST) and you want to **perform operations on its elements**.

* If you add operations directly to the element classes, they become bloated.
* Adding new operations requires modifying all element classes (violates **Open/Closed Principle**).
* On the other hand, if you move operations outside, you’ll end up with a messy chain of `if isinstance(...)` checks.

Example: A **document editor** → You may want to perform different operations on elements like `Paragraph`, `Image`, `Table`. Operations could be **rendering, exporting, spell-checking, word-counting**, etc.

---

### 2. Solution

The **Visitor Pattern** separates operations from the object structure:

* Define a `Visitor` interface with operations for each element type.
* Each element (`Paragraph`, `Image`, etc.) has an `accept(visitor)` method.
* The visitor “visits” the element and applies the correct operation.

This way:

* Adding new operations = add a new Visitor class (no change to element classes).
* Adding new element types still requires updates to visitors, but operations are cleanly separated.

---

### 3. Real-World Analogy

Think of an **insurance agent visiting different types of properties**:

* A **House** needs house insurance.
* A **Car** needs vehicle insurance.
* A **Shop** needs business insurance.

The **insurance agent (visitor)** knows what to do depending on the property type.
The **property (element)** just accepts the visitor and lets it handle the operation.

---

### 4. Python Example

Let’s implement a **Document with Elements**:

```python
from abc import ABC, abstractmethod

# Visitor Interface
class DocumentVisitor(ABC):
    @abstractmethod
    def visit_paragraph(self, paragraph):
        pass

    @abstractmethod
    def visit_image(self, image):
        pass

# Concrete Visitors
class SpellChecker(DocumentVisitor):
    def visit_paragraph(self, paragraph):
        print(f"Checking spelling in paragraph: '{paragraph.text}'")

    def visit_image(self, image):
        print("No spelling check for images.")

class Renderer(DocumentVisitor):
    def visit_paragraph(self, paragraph):
        print(f"Rendering paragraph: '{paragraph.text}'")

    def visit_image(self, image):
        print(f"Rendering image: {image.filename}")

# Element Interface
class DocumentElement(ABC):
    @abstractmethod
    def accept(self, visitor: DocumentVisitor):
        pass

# Concrete Elements
class Paragraph(DocumentElement):
    def __init__(self, text):
        self.text = text

    def accept(self, visitor: DocumentVisitor):
        visitor.visit_paragraph(self)

class Image(DocumentElement):
    def __init__(self, filename):
        self.filename = filename

    def accept(self, visitor: DocumentVisitor):
        visitor.visit_image(self)

# Client Code
if __name__ == "__main__":
    doc = [Paragraph("Hello world"), Image("photo.png"), Paragraph("Design Patterns")]

    print("Spell Check:")
    spell_checker = SpellChecker()
    for element in doc:
        element.accept(spell_checker)

    print("\nRendering:")
    renderer = Renderer()
    for element in doc:
        element.accept(renderer)
```

**Output:**

```
Spell Check:
Checking spelling in paragraph: 'Hello world'
No spelling check for images.
Checking spelling in paragraph: 'Design Patterns'

Rendering:
Rendering paragraph: 'Hello world'
Rendering image: photo.png
Rendering paragraph: 'Design Patterns'
```

---

👉 Notice:

* Elements (`Paragraph`, `Image`) only define `accept()`.
* Visitors (`SpellChecker`, `Renderer`) contain the operations.
* Adding a new operation (e.g., `WordCounter`) = just create a new Visitor class.

---

✅ Did you understand the **Visitor Pattern**?
Shall we move on to the **Interpreter Pattern** next?



Excellent — this is one of the **trickiest but most elegant** patterns, so let’s go step by step and make it *deeply intuitive*.

---

## 🧩 1️⃣ Problem Intuition — "New operations on existing objects"

Imagine you’re building an **e-book application** 📚
You have these objects:

```python
class Book: pass
class Magazine: pass
class Newspaper: pass
```

You can **read** them, fine. But one day your boss says:

> “We also need to add exporting — like print as PDF, summarize, spell check, and maybe count words — for each document type!”

Hmm... okay.
So you go and add this inside each class:

```python
class Book:
    def read(self): ...
    def export_as_pdf(self): ...
    def spell_check(self): ...
```

Then later:

> “Add translation for all document types!”

You add `translate()` in every class again.

Then later:

> “Add plagiarism check!”

You add another method everywhere.

---

### ⚠️ The problem:

Every time a **new operation** is added,
you have to **modify every class** (Book, Magazine, Newspaper…).

This violates the **Open-Closed Principle**:

> “Software entities should be open for extension, but closed for modification.”

Your classes should **not** have to change for every new operation.

---

## 💡 2️⃣ The Insight

The *operations* (PDF export, translation, spell check...)
are **different ways of visiting** the same objects.

So instead of putting all those methods *inside* the classes,
we can let **external visitors** come and “perform” the operation on them.

---

## 🧭 3️⃣ The Real-Life Analogy

Think of a **museum** 🏛️ full of different **art pieces**:

* Painting 🎨
* Sculpture 🗿
* Photograph 📸

Each art piece doesn’t know how to:

* Be described by an art critic
* Be insured by an insurance agent
* Be valued by a collector

So, instead of teaching every art piece to do all those tasks,
you let **visitors** (people) come and “visit” them.

Each visitor knows what to do **for each type** of art.

| Role      | Art Piece  | Action                   |
| --------- | ---------- | ------------------------ |
| Critic    | Painting   | Write review             |
| Collector | Sculpture  | Assign market value      |
| Insurer   | Photograph | Evaluate insurance value |

👉 Each visitor knows *how to handle each kind of object*.

That’s the **Visitor Pattern**.

---

## ⚙️ 4️⃣ Code Example (Very Intuitive)

### Step 1 — Your object structure

```python
class Book:
    def accept(self, visitor):
        visitor.visit_book(self)

class Magazine:
    def accept(self, visitor):
        visitor.visit_magazine(self)
```

---

### Step 2 — The Visitors (new operations)

```python
class PdfExporter:
    def visit_book(self, book):
        print(f"Exporting '{book.__class__.__name__}' as PDF")

    def visit_magazine(self, magazine):
        print(f"Exporting '{magazine.__class__.__name__}' as PDF")


class WordCounter:
    def visit_book(self, book):
        print(f"Counting words in {book.__class__.__name__}")

    def visit_magazine(self, magazine):
        print(f"Counting words in {magazine.__class__.__name__}")
```

---

### Step 3 — The client code

```python
documents = [Book(), Magazine()]

pdf_exporter = PdfExporter()
word_counter = WordCounter()

for doc in documents:
    doc.accept(pdf_exporter)   # Perform export
    doc.accept(word_counter)   # Perform word counting
```

🧾 Output:

```
Exporting 'Book' as PDF
Counting words in Book
Exporting 'Magazine' as PDF
Counting words in Magazine
```

---

## 🔍 5️⃣ Why This Is Powerful

| Without Visitor                                   | With Visitor                        |
| ------------------------------------------------- | ----------------------------------- |
| Every new operation requires changing all classes | You just create a new visitor class |
| Classes grow huge and messy                       | Classes stay simple and stable      |
| Violates Open-Closed principle                    | Fully respects it                   |
| Hard to maintain                                  | Easy to extend                      |

Now you can add **new visitors (operations)** freely,
without touching existing document classes.

---

## 🧱 6️⃣ Structure Recap

| Concept              | Role                                                     |
| -------------------- | -------------------------------------------------------- |
| `Element`            | Objects being visited (Book, Magazine)                   |
| `Visitor`            | Operation applied on elements (PDFExporter, WordCounter) |
| `accept(visitor)`    | Lets the visitor in                                      |
| `visit_XYZ(element)` | Visitor’s operation for a specific type                  |

---

## 💭 7️⃣ Summary (Intuition in one sentence)

> The **Visitor Pattern** lets you **add new operations** to a group of objects **without changing the objects themselves**,
> by letting those objects “accept” a **visitor** that knows how to process them.

---

## 🪄 Optional Mental Model

Think of **“double dispatch”**:

* Normally, `obj.method()` calls are *single dispatch* (depends only on object type).
* Visitor adds a *second dispatch* — it depends on both:

  1. The type of the **element** (`Book`, `Magazine`)
  2. The type of the **visitor** (`PdfExporter`, `WordCounter`)

So `book.accept(pdf_exporter)` →
calls `pdf_exporter.visit_book(book)`.

---

Would you like me to show a **real-world example in Python** (e.g., applying Visitor to parse and evaluate an Abstract Syntax Tree like a mini Python interpreter)?
