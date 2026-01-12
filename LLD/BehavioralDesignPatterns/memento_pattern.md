***<=================== move on to the Memento Pattern next ==================>***

Excellent 🙌 Let’s move on to the **Memento Pattern**.

---

### 6. Memento Pattern

---

### 1. Problem

Sometimes, you want to **save and restore the state** of an object without exposing its internal details.

Examples:

* A text editor needs **Undo/Redo**.
* A game needs **Save/Load checkpoints**.
* A drawing app needs to restore previous versions.

If you let external objects directly access internal state, you break **encapsulation**.

---

### 2. Solution

The **Memento Pattern** provides a way to:

* Capture an object’s **state** (memento).
* Store it outside the object.
* Restore it later without exposing internal implementation.

It usually involves three parts:

* **Originator** → The object whose state we want to save/restore.
* **Memento** → The snapshot of the state.
* **Caretaker** → Manages the saved states (undo/redo history).

---

### 3. Real-World Analogy

Think of a **video game checkpoint system**:

* The game (originator) saves its state at a checkpoint (memento).
* The console (caretaker) stores multiple checkpoints.
* When the player dies, the game reloads the last checkpoint without revealing all the game’s internals.

---

### 4. Python Example

Let’s implement a **Text Editor with Undo**:

```python
# Memento
class EditorMemento:
    def __init__(self, content):
        self._content = content

    def get_saved_content(self):
        return self._content

# Originator
class TextEditor:
    def __init__(self):
        self._content = ""

    def type(self, words):
        self._content += words

    def save(self):
        return EditorMemento(self._content)

    def restore(self, memento: EditorMemento):
        self._content = memento.get_saved_content()

    def show(self):
        print(self._content)

# Caretaker
class History:
    def __init__(self):
        self._history = []

    def push(self, memento: EditorMemento):
        self._history.append(memento)

    def pop(self):
        return self._history.pop() if self._history else None

# Client Code
if __name__ == "__main__":
    editor = TextEditor()
    history = History()

    editor.type("Hello, ")
    history.push(editor.save())   # save state

    editor.type("world!")
    editor.show()                 # Hello, world!

    editor.restore(history.pop()) # undo
    editor.show()                 # Hello,
```

**Output:**

```
Hello, world!
Hello,
```

---

👉 Notice:

* The `TextEditor` (Originator) keeps its logic private.
* The `EditorMemento` stores state snapshots.
* The `History` (Caretaker) manages undo history.
* Client code just uses `save()` and `restore()` without breaking encapsulation.

---

✅ Did you understand the **Memento Pattern**?
Shall we move on to the **State Pattern** next?
