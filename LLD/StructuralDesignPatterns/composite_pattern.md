***<=================ove on to the Composite Pattern next==================>***
Perfect 👌 Let’s dive into the **Composite Pattern** now, step by step.

---

### 1. **Problem**

Sometimes you want to treat **individual objects** and **groups of objects** in the same way.

Example:

* Imagine a **File System**:

  * A **File** is a single item.
  * A **Folder** can contain many files or other folders.
* You want to perform actions like `open()`, `delete()`, `display()` without caring whether it’s a single file or an entire folder.

Without a proper pattern, you’d have to write **separate logic for single objects and collections**, making the code messy.

---

### 2. **Solution**

The **Composite Pattern** organizes objects into a **tree structure** where:

* **Leaf** = individual objects (e.g., File).
* **Composite** = collection of objects (e.g., Folder).

Both implement the same **Component interface**, so the client can treat them **uniformly**.

---

### 3. **Real-world Intuition / Analogy**

Think of a **Company hierarchy**:

* An **Employee** (developer) is a single worker (Leaf).
* A **Manager** supervises multiple employees or other managers (Composite).
* You can ask *anyone* for `get_salary()` or `get_role()`, and it works the same way.

---

### 4. **Python Example**

```python
from abc import ABC, abstractmethod

# Component
class FileSystemComponent(ABC):
    @abstractmethod
    def show(self, indent=0):
        pass

# Leaf
class File(FileSystemComponent):
    def __init__(self, name):
        self.name = name

    def show(self, indent=0):
        print("  " * indent + f"File: {self.name}")

# Composite
class Folder(FileSystemComponent):
    def __init__(self, name):
        self.name = name
        self.children = []

    def add(self, component: FileSystemComponent):
        self.children.append(component)

    def show(self, indent=0):
        print("  " * indent + f"Folder: {self.name}")
        for child in self.children:
            child.show(indent + 1)

# Client usage
root = Folder("Root")
file1 = File("notes.txt")
file2 = File("todo.txt")

subfolder = Folder("Images")
subfolder.add(File("photo1.png"))
subfolder.add(File("photo2.png"))

root.add(file1)
root.add(file2)
root.add(subfolder)

root.show()
```

**Output:**

```
Folder: Root
  File: notes.txt
  File: todo.txt
  Folder: Images
    File: photo1.png
    File: photo2.png
```

Here:

* **Files** (Leaf) and **Folders** (Composite) share the same `show()` interface.
* The client doesn’t care whether it’s dealing with a single file or a folder.

---

### 5. **Check-in**

Does the **Composite Pattern** make sense with the **File System / Company hierarchy analogy** and the Python example?
Shall we move on to the **Decorator Pattern** next?
