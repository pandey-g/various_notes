***<======================== move on to the Chain of Responsibility Pattern next ==================>***

Perfect 👌 Let’s finish with the **Chain of Responsibility Pattern**.

---

### 11. Chain of Responsibility Pattern

---

### 1. Problem

Sometimes, a request must be handled by **one of several objects**, but you **don’t know in advance which object will handle it**.

* Hardcoding `if/else` to determine the handler couples your code.
* You want a flexible way to **pass the request along a chain** until someone handles it.

Example: **Customer support system**:

* A user request can be handled by **Level 1**, **Level 2**, or **Level 3 support** depending on complexity.
* The request should pass along the chain automatically until resolved.

---

### 2. Solution

The **Chain of Responsibility Pattern** creates a **chain of handler objects**:

* Each handler decides whether to handle the request or pass it to the next handler in the chain.
* The client sends the request to the first handler and doesn’t care who ultimately handles it.

---

### 3. Real-World Analogy

Think of **technical support escalation**:

* Tier 1 support tries to solve the problem.
* If they can’t, it escalates to Tier 2.
* If still unresolved, it escalates to Tier 3.
* Each tier only handles what it is capable of.

---

### 4. Python Example

Let’s implement a **support ticket system**:

```python
from abc import ABC, abstractmethod

# Handler Interface
class SupportHandler(ABC):
    def __init__(self):
        self._next_handler = None

    def set_next(self, handler):
        self._next_handler = handler
        return handler  # allow chaining

    @abstractmethod
    def handle(self, ticket_complexity):
        pass

# Concrete Handlers
class LevelOneSupport(SupportHandler):
    def handle(self, ticket_complexity):
        if ticket_complexity <= 1:
            print("Level 1 support handled the ticket.")
        elif self._next_handler:
            self._next_handler.handle(ticket_complexity)

class LevelTwoSupport(SupportHandler):
    def handle(self, ticket_complexity):
        if ticket_complexity == 2:
            print("Level 2 support handled the ticket.")
        elif self._next_handler:
            self._next_handler.handle(ticket_complexity)

class LevelThreeSupport(SupportHandler):
    def handle(self, ticket_complexity):
        if ticket_complexity >= 3:
            print("Level 3 support handled the ticket.")
        elif self._next_handler:
            self._next_handler.handle(ticket_complexity)

# Client Code
if __name__ == "__main__":
    level1 = LevelOneSupport()
    level2 = LevelTwoSupport()
    level3 = LevelThreeSupport()

    # Setup chain: Level1 -> Level2 -> Level3
    level1.set_next(level2).set_next(level3)

    tickets = [1, 2, 3, 4]

    for ticket in tickets:
        print(f"\nProcessing ticket with complexity {ticket}:")
        level1.handle(ticket)
```

**Output:**

```
Processing ticket with complexity 1:
Level 1 support handled the ticket.

Processing ticket with complexity 2:
Level 2 support handled the ticket.

Processing ticket with complexity 3:
Level 3 support handled the ticket.

Processing ticket with complexity 4:
Level 3 support handled the ticket.
```

---

👉 Notice:

* The **client only sends the request to the first handler**.
* Each handler decides to handle or forward.
* Adding new levels is easy — just add a new handler in the chain.

---

✅ That completes all **Behavioral Patterns**:

* Strategy ✅
* Observer ✅
* Command ✅
* Iterator ✅
* Mediator ✅
* Memento ✅
* State ✅
* Template Method ✅
* Visitor ✅
* Interpreter ✅
* Chain of Responsibility ✅

You now have a **complete, intuitive understanding of all 11 behavioral patterns** with examples.

If you want, I can create a **one-page summary chart for all 23 design patterns** with problems, solutions, and real-world analogies — a perfect cheat sheet for quick revision.

Do you want me to do that?
