***<=======================move on to the Builder pattern next=====================>***

Awesome 🚀 Let’s dive into the **Builder Pattern**.

---

# 4) Builder Pattern

### 1. Problem

Sometimes objects are **very complex to create**, because they:

* Have **many optional parameters**.
* Require a **step-by-step construction**.
* Need different **representations** from the same process.

👉 Example:
Imagine building a **complex SQL query** or a **meal at a fast-food restaurant**.
If you try to put everything into a single constructor with 10+ arguments, it becomes unreadable and error-prone.

---

### 2. Solution

The **Builder pattern**:

* Separates **object construction** from its **representation**.
* Lets you create complex objects **step by step**.
* The same building process can produce **different representations** (e.g., Vegetarian Meal vs Non-Veg Meal).

---

### 3. Intuition (Real-world analogy)

Think of ordering a **custom burger at McDonald’s** 🍔:

* You choose the **bun**, **patty**, **sauce**, **toppings**, step by step.
* The cashier (builder) follows a structured process.
* At the end, you get your custom burger.

👉 You don’t directly create the burger in one shot—you build it step by step.

---

### 4. Python Example

```python
# Product
class Burger:
    def __init__(self):
        self.bun = None
        self.patty = None
        self.sauce = None
        self.toppings = []

    def __str__(self):
        return f"Burger with {self.bun} bun, {self.patty} patty, {self.sauce} sauce, toppings: {', '.join(self.toppings)}"


# Builder
class BurgerBuilder:
    def __init__(self):
        self.burger = Burger()

    def set_bun(self, bun):
        self.burger.bun = bun
        return self  # chaining

    def set_patty(self, patty):
        self.burger.patty = patty
        return self

    def set_sauce(self, sauce):
        self.burger.sauce = sauce
        return self

    def add_topping(self, topping):
        self.burger.toppings.append(topping)
        return self

    def build(self):
        return self.burger


# Director (optional: orchestrates building steps)
class BurgerDirector:
    def __init__(self, builder: BurgerBuilder):
        self.builder = builder

    def make_veg_burger(self):
        return (
            self.builder
            .set_bun("whole wheat")
            .set_patty("veggie")
            .set_sauce("mayo")
            .add_topping("lettuce")
            .add_topping("tomato")
            .build()
        )

    def make_chicken_burger(self):
        return (
            self.builder
            .set_bun("sesame")
            .set_patty("chicken")
            .set_sauce("bbq")
            .add_topping("cheese")
            .add_topping("onion")
            .build()
        )


# Usage
builder = BurgerBuilder()
director = BurgerDirector(builder)

veg_burger = director.make_veg_burger()
chicken_burger = director.make_chicken_burger()

print(veg_burger)
# Burger with whole wheat bun, veggie patty, mayo sauce, toppings: lettuce, tomato

print(chicken_burger)
# Burger with sesame bun, chicken patty, bbq sauce, toppings: cheese, onion
```

👉 Key insight:

* Builder lets you create complex objects in **steps**, making code **readable and flexible**.
* You can reuse the same steps to create **different representations**.

---

✅ That’s the **Builder pattern**.
👉 Do you understand this one? Should I move on to the **Prototype pattern** next?
