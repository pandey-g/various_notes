***<=================== move on to the State Pattern next ===================>***

Great 🚀 Let’s continue with the **State Pattern**.

---

### 7. State Pattern

---

### 1. Problem

Sometimes, an object’s **behavior depends on its internal state**, and it needs to change behavior dynamically as the state changes.

If you implement this with lots of `if/else` or `switch` statements, the code becomes:

* Messy and hard to maintain
* Violates **Open/Closed Principle** (since adding a new state means modifying core logic)

Example: A **music player** → when it’s **playing**, pressing the button means *pause*. When it’s **paused**, pressing the button means *play*.

---

### 2. Solution

The **State Pattern** allows an object to **change its behavior when its state changes**, by encapsulating states into separate classes.

Key idea:

* Define a **State interface**.
* Each **Concrete State** implements behavior for that state.
* The **Context** delegates to the current state.
* Changing state = swapping the state object.

---

### 3. Real-World Analogy

Think of a **Traffic Light**:

* At **Red**, cars stop.
* At **Green**, cars go.
* At **Yellow**, cars slow down.
* The traffic light (context) delegates behavior to its current state.

You don’t need to write giant `if` statements — just switch the state object.

---

### 4. Python Example

Let’s implement a **Music Player**:

```python
from abc import ABC, abstractmethod

# State Interface
class State(ABC):
    @abstractmethod
    def press_button(self, player):
        pass

# Concrete States
class PlayingState(State):
    def press_button(self, player):
        print("Music paused")
        player.set_state(PausedState())

class PausedState(State):
    def press_button(self, player):
        print("Music playing")
        player.set_state(PlayingState())

# Context
class MusicPlayer:
    def __init__(self):
        self._state = PausedState()  # start in paused state

    def set_state(self, state: State):
        self._state = state

    def press_button(self):
        self._state.press_button(self)

# Client Code
if __name__ == "__main__":
    player = MusicPlayer()

    player.press_button()  # Music playing
    player.press_button()  # Music paused
    player.press_button()  # Music playing
```

**Output:**

```
Music playing
Music paused
Music playing
```

---

👉 Notice:

* The `MusicPlayer` doesn’t have a big `if` block to decide behavior.
* Each state (`PlayingState`, `PausedState`) knows what to do and what state to switch to next.
* Adding new states (like `StoppedState`) is easy — no modification to `MusicPlayer`.

---

✅ Did you understand the **State Pattern**?
Shall we move on to the **Template Method Pattern** next?
