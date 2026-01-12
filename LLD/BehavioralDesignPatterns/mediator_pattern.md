***<================= move on to the Mediator Pattern next ===============>***

Perfect 👌 Let’s continue with the **Mediator Pattern**.

---

### 5. Mediator Pattern

---

### 1. Problem

In complex systems, many objects often need to communicate with each other.
If every object talks to every other object directly, you get a **“spaghetti” network of dependencies** that is hard to maintain, extend, or debug.

Example: In a chat room with multiple users, if each user manages direct connections to all others, adding/removing users becomes messy.

---

### 2. Solution

The **Mediator Pattern** introduces a **mediator object** that centralizes communication:

* Colleagues (objects) don’t talk directly.
* Instead, they send messages to the **Mediator**.
* The Mediator forwards or coordinates the communication.

This reduces dependencies between objects and makes the system easier to maintain.

---

### 3. Real-World Analogy

Think of an **air traffic controller**:

* Airplanes (colleagues) don’t talk directly to each other.
* Instead, they communicate with the **control tower (mediator)**.
* The tower coordinates landings, takeoffs, and prevents collisions.
* If planes communicated directly, chaos would happen.

---

### 4. Python Example

Let’s implement a **Chat Room Mediator**:

```python
from abc import ABC, abstractmethod

# Mediator Interface
class ChatMediator(ABC):
    @abstractmethod
    def show_message(self, user, message):
        pass

# Concrete Mediator
class ChatRoom(ChatMediator):
    def show_message(self, user, message):
        print(f"[{user.get_name()}]: {message}")

# Colleague
class User:
    def __init__(self, name, mediator: ChatMediator):
        self.name = name
        self.mediator = mediator

    def get_name(self):
        return self.name

    def send(self, message):
        self.mediator.show_message(self, message)

# Client Code
if __name__ == "__main__":
    chatroom = ChatRoom()

    user1 = User("Alice", chatroom)
    user2 = User("Bob", chatroom)
    user3 = User("Charlie", chatroom)

    user1.send("Hi everyone!")
    user2.send("Hello Alice!")
    user3.send("Hey folks, what's up?")
```

**Output:**

```
[Alice]: Hi everyone!
[Bob]: Hello Alice!
[Charlie]: Hey folks, what's up?
```

---

👉 Notice:

* Users don’t send messages directly to each other.
* They go through the **ChatRoom (Mediator)**, which manages the communication.
* Adding/removing users doesn’t break existing logic.

---

✅ Did you understand the **Mediator Pattern**?
Shall we move on to the **Memento Pattern** next?
