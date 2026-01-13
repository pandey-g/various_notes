***<==================move on to the Abstract Factory pattern next==================>***

Perfect 👍 Let’s continue with the **Abstract Factory Pattern**.

---

# 3) Abstract Factory Pattern

### 1. Problem

Suppose you are building a **cross-platform GUI library**.

* On **Windows**, you need `WindowsButton`, `WindowsCheckbox`.
* On **Mac**, you need `MacButton`, `MacCheckbox`.

If you let client code decide directly which button/checkbox to create, you’ll end up with **inconsistent families** of objects:

* e.g., a **WindowsButton** with a **MacCheckbox** (which looks broken).

👉 Problems:

* Client code becomes tightly coupled to specific classes.
* Maintaining consistency across product families is difficult.

---

### 2. Solution

The **Abstract Factory pattern** provides:

* An interface for creating **families of related objects** (buttons, checkboxes).
* Ensures clients use only one family (Windows or Mac) at a time.

So instead of creating individual objects directly, client code asks the **Abstract Factory** to create them.

---

### 3. Intuition (Real-world analogy)

Think of a **furniture factory**:

* An IKEA factory produces **chairs + sofas + tables** with the **same style**.
* A Victorian factory produces the same set but in **Victorian style**.

👉 You don’t want to mix IKEA chairs with Victorian tables in the same set.
👉 The Abstract Factory ensures consistency by giving you a matching family.

---

### 4. Python Example

```python
from abc import ABC, abstractmethod

# Abstract Products
class Button(ABC):
    @abstractmethod
    def paint(self): pass

class Checkbox(ABC):
    @abstractmethod
    def paint(self): pass


# Concrete Products - Windows
class WindowsButton(Button):
    def paint(self): return "Render a Windows style button."

class WindowsCheckbox(Checkbox):
    def paint(self): return "Render a Windows style checkbox."


# Concrete Products - Mac
class MacButton(Button):
    def paint(self): return "Render a Mac style button."

class MacCheckbox(Checkbox):
    def paint(self): return "Render a Mac style checkbox."


# Abstract Factory
class GUIFactory(ABC):
    @abstractmethod
    def create_button(self) -> Button: pass

    @abstractmethod
    def create_checkbox(self) -> Checkbox: pass


# Concrete Factories
class WindowsFactory(GUIFactory):
    def create_button(self) -> Button:
        return WindowsButton()
    def create_checkbox(self) -> Checkbox:
        return WindowsCheckbox()

class MacFactory(GUIFactory):
    def create_button(self) -> Button:
        return MacButton()
    def create_checkbox(self) -> Checkbox:
        return MacCheckbox()


# Client code
def build_ui(factory: GUIFactory):
    button = factory.create_button()
    checkbox = factory.create_checkbox()
    print(button.paint())
    print(checkbox.paint())


# Usage
build_ui(WindowsFactory())
# Render a Windows style button.
# Render a Windows style checkbox.

build_ui(MacFactory())
# Render a Mac style button.
# Render a Mac style checkbox.
```

👉 Key insight:

* Client (`build_ui`) doesn’t know whether it’s using Windows or Mac objects.
* The **factory family** ensures consistency.

---

✅ That’s the **Abstract Factory pattern**.
👉 Do you understand this one? Should I move on to the **Builder pattern** next?
