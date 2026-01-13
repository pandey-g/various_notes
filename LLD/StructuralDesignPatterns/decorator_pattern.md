***<=============move on to the Decorator Pattern next=================>***
Awesome 🎉 Let’s continue with the **Decorator Pattern** step by step.

---

### 1. **Problem**

Sometimes you want to **add new behaviors or responsibilities to objects dynamically** (at runtime) without modifying their original code.

Example:

* Suppose you have a **Text Editor**.
* A basic `TextView` shows plain text.
* You might want to add features like **scrollbars**, **borders**, **syntax highlighting**, etc.
* You could subclass `TextView` into `BorderedTextView`, `ScrollableTextView`, `BorderedScrollableTextView` … but this leads to a **class explosion**.

So the problem: **How to extend functionality flexibly without modifying or subclassing excessively?**

---

### 2. **Solution**

The **Decorator Pattern** solves this by “wrapping” objects in **decorator classes** that add new behavior.

* Both the original object and decorators implement the same interface.
* Decorators contain a reference to the original object and delegate work to it, while also adding their own behavior.

Thus, you can stack decorators dynamically like layers.

---

### 3. **Real-world Intuition / Analogy**

Think of **gift wrapping**:

* The **gift (object)** is the core.
* You can add **a box**, **wrapping paper**, **a ribbon**, **a gift bag**… each layer decorates the gift.
* The core gift doesn’t change, but its **appearance/behavior** changes with each decoration.

---

### 4. **Python Example**

```python
from abc import ABC, abstractmethod

# Component
class Coffee(ABC):
    @abstractmethod
    def cost(self):
        pass

    @abstractmethod
    def description(self):
        pass

# Concrete Component
class SimpleCoffee(Coffee):
    def cost(self):
        return 5

    def description(self):
        return "Simple Coffee"

# Base Decorator
class CoffeeDecorator(Coffee):
    def __init__(self, coffee: Coffee):
        self._coffee = coffee

    def cost(self):
        return self._coffee.cost()

    def description(self):
        return self._coffee.description()

# Concrete Decorators
class MilkDecorator(CoffeeDecorator):
    def cost(self):
        return super().cost() + 2

    def description(self):
        return super().description() + ", Milk"

class SugarDecorator(CoffeeDecorator):
    def cost(self):
        return super().cost() + 1

    def description(self):
        return super().description() + ", Sugar"

# Client usage
coffee = SimpleCoffee()
print(coffee.description(), "=> $", coffee.cost())

coffee_with_milk = MilkDecorator(coffee)
print(coffee_with_milk.description(), "=> $", coffee_with_milk.cost())

coffee_with_milk_and_sugar = SugarDecorator(coffee_with_milk)
print(coffee_with_milk_and_sugar.description(), "=> $", coffee_with_milk_and_sugar.cost())
```

**Output:**

```
Simple Coffee => $ 5
Simple Coffee, Milk => $ 7
Simple Coffee, Milk, Sugar => $ 8
```

Here:

* `SimpleCoffee` is the base object.
* `MilkDecorator` and `SugarDecorator` wrap it, adding features **without modifying the original class**.
* You can mix & match decorators dynamically.

---

### 5. **Check-in**

Does the **Decorator Pattern** click for you with the **gift wrapping / coffee example**?
Shall we move on to the **Facade Pattern** next?
