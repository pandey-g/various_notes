***<================ move on to the Template Method Pattern next ===============>***

Perfect 👍 Let’s move on to the **Template Method Pattern**.

---

### 8. Template Method Pattern

---

### 1. Problem

Sometimes, you have an algorithm where the **overall structure is fixed**, but some **steps can vary** depending on the subclass.

If you implement the full algorithm in one class, you’ll end up duplicating code when different variations are needed.
This violates **DRY (Don’t Repeat Yourself)** and makes code hard to extend.

Example: A **data processing pipeline** → steps might be:

1. Load data
2. Process data
3. Save results

The skeleton is the same, but the details of each step depend on the type of data (CSV, JSON, XML, etc.).

---

### 2. Solution

The **Template Method Pattern** defines the **skeleton of an algorithm in a base class**, but lets subclasses **override specific steps** without changing the algorithm’s structure.

Key idea:

* The **template method** is defined in the abstract base class.
* Subclasses provide their own implementation for the variable parts.

---

### 3. Real-World Analogy

Think of **making tea vs. making coffee**:

* Steps: Boil water → Brew drink → Pour into cup → Add condiments.
* The skeleton (boil, pour, add) is the same.
* The only difference: **tea leaves vs. coffee grounds**.

So we define the **template** for making a hot drink, and subclasses fill in the details.

---

### 4. Python Example

Let’s implement a **Data Parser** example:

```python
from abc import ABC, abstractmethod

# Abstract Class with Template Method
class DataParser(ABC):

    # Template Method
    def parse_data(self):
        self.read_data()
        self.process_data()
        self.save_data()

    @abstractmethod
    def read_data(self):
        pass

    @abstractmethod
    def process_data(self):
        pass

    def save_data(self):
        print("Saving processed data to database.")

# Concrete Implementations
class CSVParser(DataParser):
    def read_data(self):
        print("Reading data from CSV file.")

    def process_data(self):
        print("Processing CSV data.")

class JSONParser(DataParser):
    def read_data(self):
        print("Reading data from JSON file.")

    def process_data(self):
        print("Processing JSON data.")

# Client Code
if __name__ == "__main__":
    csv_parser = CSVParser()
    json_parser = JSONParser()

    print("CSV Parsing:")
    csv_parser.parse_data()

    print("\nJSON Parsing:")
    json_parser.parse_data()
```

**Output:**

```
CSV Parsing:
Reading data from CSV file.
Processing CSV data.
Saving processed data to database.

JSON Parsing:
Reading data from JSON file.
Processing JSON data.
Saving processed data to database.
```

---

👉 Notice:

* `parse_data()` is the **template method** (fixed algorithm).
* Subclasses (`CSVParser`, `JSONParser`) provide specific implementations for steps.
* Adding a new parser (e.g., XML) is simple — just implement the abstract methods.

---

✅ Did you understand the **Template Method Pattern**?
Shall we move on to the **Visitor Pattern** next?
