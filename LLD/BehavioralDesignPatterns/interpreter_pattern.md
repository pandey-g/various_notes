***<=================== move on to the Interpreter Pattern next ===============>***

Perfect! Let’s continue with the **Interpreter Pattern** 🚀

---

### 10. Interpreter Pattern

---

### 1. Problem

Sometimes, you need to **interpret a language, expression, or grammar** repeatedly.

* Hardcoding rules for parsing and evaluating expressions can become messy and difficult to maintain.
* You want a **flexible, extensible way** to define grammar and interpret expressions.

Example: A **simple calculator** that supports expressions like `"5 + 3"` or `"10 - 2 + 4"`.
You want a design that can easily handle new operations like multiplication or parentheses.

---

### 2. Solution

The **Interpreter Pattern** defines:

* A **grammar representation** for the language (classes for operations, numbers, etc.).
* An **interpret()** method in each class to evaluate the expression.

Key idea:

* Each element of the grammar knows **how to interpret itself**.
* Complex expressions are built by composing simple expressions.

---

### 3. Real-World Analogy

Think of a **mathematical expression evaluator**:

* A **Number** knows its value.
* An **Addition** knows how to add two expressions.
* You can combine them recursively: `(5 + 3) + 2`.
* Each part “interprets” itself to produce the final result.

---

### 4. Python Example

Let’s implement a **simple arithmetic expression interpreter**:

```python
from abc import ABC, abstractmethod

# Abstract Expression
class Expression(ABC):
    @abstractmethod
    def interpret(self):
        pass

# Terminal Expressions
class Number(Expression):
    def __init__(self, value):
        self.value = value

    def interpret(self):
        return self.value

# Non-terminal Expressions
class Add(Expression):
    def __init__(self, left: Expression, right: Expression):
        self.left = left
        self.right = right

    def interpret(self):
        return self.left.interpret() + self.right.interpret()

class Subtract(Expression):
    def __init__(self, left: Expression, right: Expression):
        self.left = left
        self.right = right

    def interpret(self):
        return self.left.interpret() - self.right.interpret()

# Client Code
if __name__ == "__main__":
    # Represents the expression: (5 + 3) - 2
    expr = Subtract(Add(Number(5), Number(3)), Number(2))
    result = expr.interpret()
    print("Result:", result)
```

**Output:**

```
Result: 6
```

---

👉 Notice:

* `Number` is a terminal expression (leaf node).
* `Add` and `Subtract` are non-terminal expressions (internal nodes).
* The structure can be easily extended to support **Multiply, Divide, or parentheses**.
* Each expression knows **how to interpret itself**.

---

✅ Did you understand the **Interpreter Pattern**?
Shall we move on to the **Chain of Responsibility Pattern** next?
