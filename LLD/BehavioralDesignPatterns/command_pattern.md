***<=============== move on to the Command Pattern next ================>***

Perfect 👌 Let’s dive into the **Command Pattern**.

---

### 3. Command Pattern

---

### 1. Problem

Sometimes, you want to **encapsulate a request as an object** so you can:

* Queue it
* Log it
* Undo/redo it
* Execute it later
* Pass it around as a parameter

If you directly call methods on objects, you tightly couple the invoker (the one asking) with the receiver (the one doing the work).
This makes it hard to support features like **undo, macro recording, or deferred execution**.

Example: A remote control for a TV → You press buttons (invoker), but you don’t want the remote to know exactly how the TV executes “on” or “off.”

---

### 2. Solution

The **Command Pattern** turns a request into a separate object (a **Command**).

* The **Invoker** → asks for the command to be executed.
* The **Command** → encapsulates the action (knows which method to call).
* The **Receiver** → the real object that does the work.

This decouples **what to do** from **when/how it is triggered**.

---

### 3. Real-World Analogy

Think of a **restaurant waiter**:

* You (the client) tell the waiter what you want.
* The waiter writes it down as an **order slip (command)**.
* The chef (receiver) prepares the food.
* The waiter (invoker) doesn’t need to know how the food is cooked.
* You can also queue orders, cancel them, or redo them.

---

### 4. Python Example

Let’s implement a **Remote Control for a Light**:

```python
from abc import ABC, abstractmethod

# Command Interface
class Command(ABC):
    @abstractmethod
    def execute(self):
        pass

    @abstractmethod
    def undo(self):
        pass

# Receiver
class Light:
    def on(self):
        print("Light is ON")

    def off(self):
        print("Light is OFF")

# Concrete Commands
class LightOnCommand(Command):
    def __init__(self, light: Light):
        self.light = light

    def execute(self):
        self.light.on()

    def undo(self):
        self.light.off()

class LightOffCommand(Command):
    def __init__(self, light: Light):
        self.light = light

    def execute(self):
        self.light.off()

    def undo(self):
        self.light.on()

# Invoker
class RemoteControl:
    def __init__(self):
        self._history = []

    def press(self, command: Command):
        command.execute()
        self._history.append(command)

    def press_undo(self):
        if self._history:
            command = self._history.pop()
            command.undo()

# Client Code
if __name__ == "__main__":
    light = Light()

    light_on = LightOnCommand(light)
    light_off = LightOffCommand(light)

    remote = RemoteControl()

    remote.press(light_on)   # Light is ON
    remote.press(light_off)  # Light is OFF

    print("\nUndoing last command...")
    remote.press_undo()      # Undo -> Light is ON
```

**Output:**

```
Light is ON
Light is OFF

Undoing last command...
Light is ON
```

---

👉 This way, the **remote (Invoker)** doesn’t need to know how the light works.
It just executes commands. And we can easily extend this to other devices (Fan, TV, Music Player) without changing the remote logic.

---

✅ Did you understand the **Command Pattern**?
Shall we move on to the **Iterator Pattern** next?
