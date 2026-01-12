***<============  move on to the Prototype pattern next?    =================>***

Perfect 👍 Let’s move to the **Prototype Pattern** — the last of the **Creational patterns**.

---

# 5) Prototype Pattern

### 1. Problem

Sometimes creating an object is:

* **Expensive** (e.g., building a large data structure, loading from a database).
* **Complex** (many configurations/initializations).
* Or you simply need **a copy of an existing object** but with small modifications.

👉 If you use `new` (or constructor) every time, you pay the full cost of creation again and again.

---

### 2. Solution

The **Prototype pattern** solves this by:

* Creating new objects by **cloning an existing object (prototype)**.
* Instead of instantiating from scratch, you copy an existing object and adjust it as needed.

This way:

* You **reuse** the initialization.
* You can create **variations quickly**.

---

### 3. Intuition (Real-world analogy)

Think of a **photocopy machine** 📄:

* Instead of rewriting a 10-page form from scratch every time, you **make a copy of an existing form**.
* Then, you just **fill in the blanks** (customize the copy).

👉 The prototype is your master copy.

---

### 4. Python Example

```python
import copy

class Prototype:
    def clone(self):
        return copy.deepcopy(self)


class Document(Prototype):
    def __init__(self, title, content, metadata=None):
        self.title = title
        self.content = content
        self.metadata = metadata or {}

    def __str__(self):
        return f"Document: {self.title}, Content: {self.content}, Metadata: {self.metadata}"


# Usage
original = Document("Report", "Annual financial data", {"author": "Alice"})
print("Original:", original)

# Create a clone and modify it
copy1 = original.clone()
copy1.title = "Report (Copy)"
copy1.metadata["author"] = "Bob"

print("Cloned:", copy1)
print("Original still intact:", original)
```

👉 Output:

```
Original: Document: Report, Content: Annual financial data, Metadata: {'author': 'Alice'}
Cloned: Document: Report (Copy), Content: Annual financial data, Metadata: {'author': 'Bob'}
Original still intact: Document: Report, Content: Annual financial data, Metadata: {'author': 'Alice'}
```

Key insight:

* `original` stays intact.
* You can create multiple modified versions **without reconstructing from scratch**.

---

✅ That’s the **Prototype pattern**.

👉 Do you understand this one?
Shall I now move on to the **Structural patterns** (starting with **Adapter**) or would you like me to recap all 5 Creational patterns first?
