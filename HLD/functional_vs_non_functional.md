***<========== what are functional and non - functional requirements in system design ===========>***
Great question — this is a **core concept in system design** (and software engineering in general). Let’s break it down clearly 👇

---

## 🧩 **Functional Requirements**

Functional requirements describe **what the system should do** — i.e., **its specific behaviors, features, and functions**.
They define **the system’s capabilities** from the user’s or business’s perspective.

### 🔹 Examples:

1. **User Authentication:**

   * The system should allow users to sign up, log in, and log out.
2. **Data Management:**

   * The system should let users create, read, update, and delete records.
3. **Payment Processing:**

   * The system should process credit card payments via Stripe API.
4. **Notifications:**

   * The system should send email notifications after order confirmation.
5. **Search:**

   * The system should allow users to search products by name or category.

### 🧠 Think of functional requirements as:

> “**What should the system do?**”

---

## ⚙️ **Non-Functional Requirements (NFRs)**

Non-functional requirements describe **how the system should perform** — i.e., **the quality attributes, constraints, or performance goals**.
They don’t add new features but define how the system behaves under various conditions.

### 🔹 Examples:

1. **Performance:**

   * The system should handle 10,000 requests per second.
2. **Scalability:**

   * The system should scale horizontally as the number of users increases.
3. **Availability:**

   * The system should have 99.99% uptime.
4. **Security:**

   * All passwords must be hashed and stored securely.
5. **Usability:**

   * The interface should load within 2 seconds on standard broadband.
6. **Maintainability:**

   * The codebase should follow clean architecture and be easily extensible.
7. **Reliability:**

   * The system should recover automatically from hardware failures.

### 🧠 Think of non-functional requirements as:

> “**How well should the system perform its functions?**”

---

## 🧮 Quick Comparison Table

| Aspect         | Functional Requirements         | Non-Functional Requirements                       |
| -------------- | ------------------------------- | ------------------------------------------------- |
| **Focus**      | What the system should do       | How the system should behave                      |
| **Type**       | Features and operations         | Quality attributes and constraints                |
| **Example**    | “User can upload files.”        | “File upload must complete within 3 seconds.”     |
| **Validation** | Tested via functionality tests  | Tested via performance, security, or stress tests |
| **Dependency** | Directly affects business logic | Affects user satisfaction and system stability    |

---

## 💡 In Real System Design

When designing a large system (like YouTube or an e-commerce app):

* **Functional requirements** → define what features you’ll build.
* **Non-functional requirements** → guide your architecture, choice of tech stack, and infrastructure setup (e.g., caching, load balancing, database replication).

---

Would you like me to give a **real-world example** (like designing Netflix or an online banking system) showing both types of requirements side by side?
ß